# Apache_Mahout

Steps followed (as per https://aws.amazon.com/blogs/big-data/building-a-recommender-with-apache-mahout-on-amazon-elastic-mapreduce-emr/)

first crated an EMR Cluster with default settings.

get MovieLens data

wget http://files.grouplens.org/datasets/movielens/ml-1m.zip
unzip ml-1m.zip

cat ml-1m/ratings.dat | sed 's/::/,/g' | cut -f1-3 -d, > ratings.csv

hadoop fs -put ratings.csv /ratings.csv

mahout recommenditembased --input /ratings.csv --output recommendations --numRecommendations 10 --outputPathForSimilarityMatrix similarity-matrix --similarityClassname SIMILARITY_COSINE

hadoop fs -ls recommendations
hadoop fs -cat recommendations/part-r-00000 | head

Building a service to get recommendations

Get Twisted, and Klein and Redis modules for Python.

sudo easy_install twisted
sudo easy_install klein
sudo easy_install redis

Install Redis and start up the server.

wget http://download.redis.io/releases/redis-2.8.7.tar.gz
tar xzf redis-2.8.7.tar.gz
cd redis-2.8.7
make
./src/redis-server &
