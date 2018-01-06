---
layout: post
title:  Machine learning to rapidly search for the correct bitcoin block header nonce  - response to Arun Subramanian  
---

This post assumes familiarity with bitcoin mining, if you don't - please read the relevant section in [bitcoin developers reference](https://bitcoin.org/en/developer-reference#block-headers) or [bitcoin wiki](https://en.bitcoin.it/wiki/Block_hashing_algorithm).

Earlier todat I've found [this article](http://carelesslearner.blogspot.co.il/2015/03/machine-learning-to-quickly-search-for.html) and decided to check his atitude.
The first thing to do - is to run the supplied code, and check the results:

    Training set accuracy : 0.999698333333
    Test set accuracy : 0.785468333333

Seems realy good, (except that the H-U-G-E memory requirments of the code - it took around GB of RAM).
Especialy when counting the time it takes (less than a second per block, on a cpu), seems lke a promising mining technic!

Now for real world tests: let's supply it with a data from [latest block](https://blockexplorer.com/block/0000000000000000007ed8ee7b255faa0e1195fad9c2f344c32954c00ae08ac6):

    >>> blk={'nonce': 2262441414, 'ver': 536870912, 'prev_block': u'00000000000000000040fecec1b3a13190738874b1932a485abffc5027197e11', 'mrkl_root': u'c3746a465c271e7b695b6f2bc118d5e5186be864b7898b3265099e21e7e211e5', 'time': 1515182439, 'bits': '180091c1'}
    
This code fails - because it's built only for singel digit version, so I try for [another block](https://blockexplorer.com/block/00000000000000000a6cef14328dbca41ea7db8483e50e78be23b9a6bd238ff7) from a year ago, with version 2:

    >>> blk={'nonce': 4251983269, 'ver': 2, 'prev_block': u'000000000000000006aec99370453affdef4f1b76f926bfe555b6b840212f75f', 'mrkl_root': u'4c8c8e1c6e5af4d32e58a23716995cf0e465e79c5794a4af96108fece4cfd52b', 'time': 1415141845, 'bits': '181e8dc0'}
    >>> data=pd.DataFrame(make_df([blk]))
    >>> nX = data[X]
    >>> nY = data[Y]
    >>> res = clf.predict(nX)
    >>> res
    array([1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1], dtype=int64)
    >>> sum([long(res[i]<<i) for i in range(len(res))])
    16640800554416594941L
    >>> pack("Q", _).encode('hex')
    'fddf1f00f0ffefe6'
    >>> pack("Q", blk['nonce']).encode('hex')
    'a51d70fd00000000'

Do you understand why is the need for 64 bit integer when nonce is only 32 bit? can you derive the real nonce from the given result? I didn't understood that yet, but let's focus:
It's not the original nonce, but the most importent is the sha256d of the results, since I don't know wich nonce to count, I tried it with both - upper and lower 32 bits:

    >>> def raw_block(version, hashPrevBlock, hashMerkleRoot, Time, Bits, Nonce):
      	return pack("I", version)+ hashPrevBlock.decode('hex')[::-1]+ hashMerkleRoot.decode('hex')[::-1] + pack("I", Time) + Bits.decode('hex')[::-1]+ pack("I", Nonce)
    >>> sha256(sha256(raw_block(blk['ver'], blk['prev_block'], blk['mrkl_root'], blk['time'], blk['bits'], blk['nonce'])).digest()).hexdigest()
    'f78f23bda6b923be780ee58384dba71ea4bc8d3214ef6c0a0000000000000000'
    >>> sha256(sha256(raw_block(blk['ver'], blk['prev_block'], blk['mrkl_root'], blk['time'], blk['bits'], 0xfddf1f00)).digest()).hexdigest()
    'c4761ac10b639eb9e130ba270a9642eca3c74a9c44b4aef0b7d682e3fd5facc4'
    >>> sha256(sha256(raw_block(blk['ver'], blk['prev_block'], blk['mrkl_root'], blk['time'], blk['bits'], 0xf0ffefe6)).digest()).hexdigest()
    'a1c682a45b9bb5f91fe65e4c65844371d24f419fd786f8bd2f00df69967668e0'
    >>> sha256(sha256(raw_block(blk['ver'], blk['prev_block'], blk['mrkl_root'], blk['time'], blk['bits'], 0xe6effff0)).digest()).hexdigest()
    '93765db79ccae22736a92c1ea3c4f576486c14254df04e2970f909dbd14a3696'
    >>> sha256(sha256(raw_block(blk['ver'], blk['prev_block'], blk['mrkl_root'], blk['time'], blk['bits'], 0x001fdffd)).digest()).hexdigest()
    'c0dba5d66a4678d4c76f2e983544d98cd6262725f7a6b661eaf99eecb425012a'

None of the options returns legitimate block.

### having a closer look at the code

This guy realy took few of bitcoin first blocks, from block 1 to block 49799, than split it to training & test set, than train random forest on it - and hopa - we have an efficient machine learning bitcoin miner!

realy?

Let's have a look at the code:

    # loading the data
    def load_data(filename):
        with open(filename, 'rb') as f:
            return pickle.load(f)
    t = load_data('D:\\Downloads\\bitcoinheaders.pkl')
    # use only 20000 + 4000 rows instead of the ~49000 - makes the process faster
    train = t[0:20000]
    test = t[21000:25000]

until here everything seems good. let's have a look at the data we got:

    >>> t[0]
    {'nonce': 1750778392, 'ver': 1, 'prev_block': u'00000000322a8c1985070452413d9c1c5e81c94d0597dac1015b8778087877c2', 'mrkl_root': u'b2b138021185c50cae4021f0c6aa39e2db8e7d6c5fd0d57599aa7c719e85af68', 'time': 1252019489, 'bits': '1d00ffff'}
    >>> st=sorted(t, key = lambda x: x['time'])
    >>> st[0]
    {'nonce': 2083236893, 'ver': 1, 'prev_block': u'0000000000000000000000000000000000000000000000000000000000000000', 'mrkl_root': u'4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b', 'time': 1231006505, 'bits': '1d00ffff'}
    >>> st[-1]
    {'nonce': 161192308, 'ver': 1, 'prev_block': u'000000001420ca2dc36ac912e374b1145299854134e15b7bcf71818e9b957672', 'mrkl_root': u'd9779ebd56c2ab520b9fbc391d1952b70ccc46e28293ac42aafd72c8405c7c3b', 'time': 1270843629, 'bits': '1c2a1115'}
    
checking that - those are the real first block blocks, let's get back to the code:

    def get_nonce(n):
        r = random.randint(0, 4294967296)
        while r == n:
            r = random.randint(0, 4294967296)
        if r < n:
            l = 0
        elif r > n:
            l = 1
        return (r, l)

    def make_df(data_dict):
        ml_df = []
        row = []
        hex_int_dict = {'0':0, '1':1, '2':2, '3':3, '4':4, '5':5, 
                        '6':6, '7':7, '8':8, '9':9, 'a': 10, 
                        'b': 11, 'c':12, 'd':13, 'e':14, 'f':15}
        
        for i in data_dict:
            try:
                header = [str(i['ver'])]
                header.extend(list(i['prev_block']))
                header.extend(list(i['mrkl_root']))
                header.extend(list(str(i['time']).zfill(10))) 
                header.extend(list(str(i['bits'])))
                nonce = int(i['nonce'])

                for n in range(150):
                    rand_test_nonce, label = get_nonce(nonce)
                    row = list(header)
                    row = [hex_int_dict[r] for r in row]
                    row.extend([rand_test_nonce, label, nonce])
                    ml_df.append(row)
            except:
                continue
        return ml_df
        
What did I just read? it takes any block in the list, convert it to bytes array - without nonce, append a random number, a bit telling whether the random is greater or less than the nonce, and -  the nonce itself. This is done 150 times for each block - to give 150 records for training / testing from each block.

The data format seems like a realy bad choice - what should number order be relevant when we are dealing with hashes and binary data?

The data choice format is also the explanation for the huge memory requirments - 150(records/block)*150(numbers/record)*25k(blocks)*8(bytes/number) - here are my 8GB.

But keep questions and optimizations for another day - let's continue to see if it works:
    
    # Convert it to dataframe in a format that sklearn can use
    ml_df = pd.DataFrame(make_df(train))
    ml_df_test = pd.DataFrame(make_df(test))

    # Split the raw data into test and training sets
    X = ml_df.columns[0:148]
    Y = ml_df.columns[148]
    X_train = ml_df[X]
    Y_train = ml_df[Y]
    X_test = ml_df_test[X]
    Y_test = ml_df_test[Y]
    
Have you noticed something? the chice of X and Y? the only thing Y is trying to predict - is if the last X is less or more than the nonce.

    def train_randomforest(X_train, Y_train):
        clf = RandomForestClassifier(n_jobs=10)
        clf = clf.fit(X_train, Y_train)
        return clf
    clf = train_randomforest(X_train, Y_train)
    
What happens now? the classifier learns but what is it trying to predict? Nothing helpfull - just if the number ("the guess for nonce") will be bigger or less than the given nonce.

So what happened in the opening of the post?

It gave realy good results on both the situations, and for different reasons:

For the training set - I can't be sure, but I guess it's just fitting - the classifier learned that parameters really good.
But for the test result - what about them? We only need to guess what the trivial algorithm will do - if the guess is less than half the maximum it will ask for bigger number, and if it's less - ask for less, yeh?
This should give the right answer 75% of the time, so what happened in the other 3.5%? Those are related to [distribiution of nonce numbers](https://bitslog.wordpress.com/2013/09/03/new-mystery-about-satoshi/), since the miners starts from the first number, there is a better chance of choosing a small number, so it's better to assume smaller numbers - and so it can get more than 75% accuracy.

Now we can understand what the numbers we got at the begining means - the algo predicted that the nonce is going to be bigger than most of the numbers in the 150 random numbers we got from `make_df`, the classifier was trying to tell us something but we didn't understand.

Anyway, let's test it assumptions:

    >>> blk['nonce']
    4251983269L
    >>> data[147][nY == res]
    13      140957459
    16     4279704161
    21      279401615
    52       82540725
    56      132364967
    100      75027343
    123      90063984
    124     141822534
    Name: 147, dtype: int64
    >>> nY[nY == res]
    13     0
    16     1
    21     0
    52     0
    56     0
    100    0
    123    0
    124    0
    Name: 148, dtype: int64
     
Here the classifier assumptions failed totally - the prediction was right only in 8 out of 150 tests, this is something I can't explain - maybe I don't read the data right... Check it another day..

   




