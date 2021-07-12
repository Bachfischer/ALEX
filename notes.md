Connection to SOSD Benchmark Server:

`ssh -i .ssh/google_compute_engine bachfischer@35.239.92.28`

`./build/benchmark -r 1 ./data/books_200M_uint32 ./data/books_200M_uint32_equality_lookups_10M --pareto --only ALEX`

Note:

Running SOSD benchmark requires at least 32GB of RAM (see https://github.com/learnedsystems/SOSD/issues/9)

RESULT: ALEX,1,525.138,3464576376,79312627769,BinarySearch
RESULT: ALEX,4,526.452,866300360,15963505249,BinarySearch
RESULT: ALEX,8,515.49,433219884,6733432636,BinarySearch
RESULT: ALEX,16,543.581,216669964,2802103456,BinarySearch
RESULT: ALEX,32,624.082,108407724,1295095967,BinarySearch
RESULT: ALEX,64,642.192,54274284,602040474,BinarySearch
RESULT: ALEX,128,653.302,27207040,283432945,BinarySearch
RESULT: ALEX,256,667.77,13610220,124225189,BinarySearch
RESULT: ALEX,512,695.47,6844340,59067938,BinarySearch
RESULT: ALEX,1024,737.501,3453384,28976822,BinarySearch
RESULT: ALEX,2048,786.078,1729776,12904752,BinarySearch



In terms of performance, ALEX has a couple of known limitations:

## Attack 1:

The premise of ALEX is to model the key distribution using a collection of linear regressions. Therefore, ALEX performs poorly when the key distribution is difficult to model with linear regressions, i.e., when the key distribution is highly nonlinear at small scales. A possible future research direction is to use a broader class of modeling techniques (e.g., also consider polynomial regression models).

### Task 1:  Establish baseline results with easy-to-model key distribution

#### Out-of-box dataset from ALEX authors
./build/benchmark \
--keys_file=/data/longitudes-200M.bin \
--keys_file_type=binary \
--init_num_keys=10000000 \
--total_num_keys=20000000 \
--batch_size=1000000 \
--insert_frac=0.5 \
--lookup_distribution=zipf \
--print_batch_stats


#### Custom dataset (random)
./build/benchmark \
--keys_file=/data/random_500000.csv \
--keys_file_type=text \
--init_num_keys=250000 \
--total_num_keys=500000 \
--batch_size=100000 \
--insert_frac=0.5 \
--lookup_distribution=uniform \
--print_batch_stats

### Task 2: Create manually-crafted dataset with nonlinear key distribution

#### Custom dataset (poisoned)

./build/benchmark \
--keys_file=/data/poisoning_keys_2000.csv \
--keys_file_type=text \
--init_num_keys=1000 \
--total_num_keys=2000 \
--batch_size=1000 \
--insert_frac=0.5 \
--lookup_distribution=uniform \
--print_batch_stats



## Attack 2:

ALEX can have poor performance in the presence of extreme outlier keys, which can cause the key domain and ALEX's tree depth to become unnecessarily large (see Section 5.1 of our paper). A possible future research direction is to add special logic for handling extreme outliers, or to have a modeling strategy that is robust to sparse key spaces.