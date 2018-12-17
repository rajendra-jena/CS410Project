
# CS-410 Project
Submission by Rajendra Jena (rjena2@illinois.edu)


Worked individually on the project.

# Name: Hot Stock Pick

# Overview:
This will help to gather the stock market data from various sources and recommend and help users to decide whether it is a good time to buy his favorite stock or not.

# Implementation:

This project has been implemented based on the Sparkler. Sparkler is a tool, which can crawl the website using the advanced distributed computing technologies.

The main advantage of Sparkler tool is:

1) Its based on distributed computing technologies.
2) Its highly scalable.
3) Runs with high performance.
4) It provides fault tolerance.
5) It Supports complex and real-time analytics.
6) Streams out the content in real-time
7) Java Script Rendering
8) It’s a distributed crawler that can scale horizontally.
9) Easy to deploy and easy to use
10) ML based analysis
11) It provides real-time progress reports
12) It supports Batch crawling
13) It used Apache Solr as crawl database, so we can utilize the
rich search features of solr.
14) It Stream crawled content through Apache Kafka.

Once we finished crawling data, we can find our automatically indexed data, in the solr database, named crawldb.
Then based on our requirement we can query the data to analyze and get the meaningful information from the collected documents.

Querying the database is simple. The exposed REST interface give us the flexibility to query the documents based on various criteria.
We can re-build the index of the retrieved documents, on the run time, again and again using the exposed REST interface.
By have two in-build clustering algorithms for the documents. Naive Bayes and KNN. Either one can be configured to be in action in the configuration file.


Setup and Installation:
Pre-requisite:
- Java 1.8
- Sparkler
- Apache Solr

Java 1.8
Download and install java 1.8, if not already is there.

Sparkler:
Download and build from git repository.
1) Create a directory in your system, named sparkler.      
mkdir ~/sparkler      
cd sparkler       

2) Download and unzip the zip file for sparkler or clone and checkout the below repository.
https://github.com/USCDataScience/sparkler

3) Once source code are in your local system, go to the sparkler directory. You should see below directories under it.
ls -ltr       

 sparkler-plugins
 sparkler-deployment
 scalastyle_config.xml
 pom.xml
 eclipse-codeformat.xml
 docs
 conf
 bin
 Release-Checklist.md
 README.md
 LICENSE
 sparkler-tests-base
 sparkler-api
 sparkler-app
 sparkler-ui

4) Make the build. 
(Prerequisite is latest maven has been installed in your system. To check, install and run maven pls. refer the page:
https://maven.apache.org/)       
mvn clean package       

5) Build the dashboard UI for sparkler.      
cd sparkler-ui      
mvn clean package          

6) Go to the parent sparkler directory and download apache solr.        
cd ~/sparkler          
Download Solr Binary          
For Mac : curl -O http://archive.apache.org/dist/lucene/solr/7.5.0/solr-7.5.0.tgz       
For other: wget "http://archive.apache.org/dist/lucene/solr/7.5.0/solr-7.5.0.tgz"  # or pick your version and mirror    

7) Extract Solr      
tar xvzf solr-7.5.0.tgz         

8) Add crawldb config sets        
cd solr-7.5.0/       
cp -rv ~/sparkler/build/conf/solr/crawldb server/solr/configsets/        

9) Setup the sparkler dashboard.        
cp -r ~/sparkler/sparkler-ui/target/sparkler-ui-*.war ~/sparkler/solr-7.5.0/server/solr-webapp/sparkler          
cp ~/sparkler/build/conf/solr/sparkler-jetty-context.xml ~/sparkler/solr-7.5.0/server/contexts/         

10) Start solr in cloud mode       
bin/solr -e cloud          
- select 1 node only for testing or choose the number of nodes as you like.
- choose default port 8983 for the node, else choose your own port. For multiple nodes, choose different ports as you like.
- give the new collection name as "crawldb"
- choose the default options for shards and replicas by entering enter.
- for configuration, type "crawldb" and enter.
The solr should start, running the zookeeper at localhost:9983, and collection crawldb.

11) check solr is running and sparkler dashboard is running.         
open the pages in separate browser windows:    

http://localhost:8983/solr       
http://localhost:8983/banana/       


11) Create a file to seed the URLs    

crate a file "seedfile.txt", and put the URL's you want to crawl.         
eg. add the below lines, to the seedfile.txt.     

https://seekingalpha.com/      
https://stocktwits.com/      
https://finance.yahoo.com/      

12) Inject Seed URLs     

java -jar sparkler-app-0.*-SNAPSHOT.jar inject -sf seedfile.txt         
or          
bin/sparkler.sh inject -id sparkler-job-123456 \          
      -su https://seekingalpha.com/ -su https://stocktwits.com/ -su https://finance.yahoo.com/    

13) run crawl.      
bin/sparkler.sh crawl -id sparkler-job-123456  -m local[*] -i 10        

Once finished, we are done with crawl the web and out crawldb database is ready to be used.
The documents are already indexed and clustered.

14) We can find the details of our crawling results from both solr ui and banana dashboard.

http://localhost:8983/solr        
http://localhost:8983/banana/         

15) Search the solr index, from its REST interface as per your requirement to get the meaningful analysis and information.

e.g.      


Usage:      
Once setup has been done, application is up and running, use the REST interface of the Solr, to query the document and index.


Future work:        
1) We need to implement and integrate Kafka, with the tool. By this we will get real time stock market data to flow to our system.
And we can get real time picture of our interest data, and take a decision at that point of time.

References:       
https://github.com/USCDataScience/sparkler        
https://spark.apache.org/         
http://nutch.apache.org/       
https://tika.apache.org/         
http://lucene.apache.org/         
http://lucene.apache.org/solr/       
https://maven.apache.org/       
 

