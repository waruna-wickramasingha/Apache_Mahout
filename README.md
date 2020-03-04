# Apache_Mahout

Steps followed (as per https://aws.amazon.com/blogs/big-data/building-a-recommender-with-apache-mahout-on-amazon-elastic-mapreduce-emr/)

1 first crated an EMR Cluster with default settings.

2 get MovieLens data

wget http://files.grouplens.org/datasets/movielens/ml-1m.zip
unzip ml-1m.zip

Convert ratings.dat, trade “::” for “,”, and take only the first three columns:
cat ml-1m/ratings.dat | sed 's/::/,/g' | cut -f1-3 -d, > ratings.csv

Put ratings file into HDFS:
hadoop fs -put ratings.csv /ratings.csv

3 Run the recommender job:

mahout recommenditembased --input /ratings.csv --output recommendations --numRecommendations 10 --outputPathForSimilarityMatrix similarity-matrix --similarityClassname SIMILARITY_COSINE

4 Look for the results in the part-files containing the recommendations:

hadoop fs -ls recommendations
hadoop fs -cat recommendations/part-r-00000 | head

Building a service to get recommendations

1 Get Twisted, and Klein and Redis modules for Python.

sudo easy_install twisted

sudo easy_install klein

sudo easy_install redis

2 Install Redis and start up the server.

3 Build a web service that pulls the recommendations into Redis and responds to queries.
Put the following into a file, e.g., “hello.py”

wget http://download.redis.io/releases/redis-2.8.7.tar.gz
tar xzf redis-2.8.7.tar.gz
cd redis-2.8.7
make
./src/redis-server &

4 Start the web service.
twistd -noy hello.py &

5 Test the web service to get recommendarions for ex:- user id = “37”,
curl localhost:8080/37

result:
The recommendations for user 37 are [7:5.0,2088:5.0,2080:5.0,1043:5.0,3107:5.0,2087:5.0,2078:5.0,3108:5.0,1042:5.0,1028:5.0]
 
done!

