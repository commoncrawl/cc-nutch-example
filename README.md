Example Usage of Common Crawl's Fork of Apache Nutch to Crawl and Write WARC files
==================================================================================

A short description how to set up [Common Crawl's Fork](https://github.com/commoncrawl/nutch/)
of [Apache Nutch](https://nutch.apache.org/) for crawling and to store the
crawled content in WARC files.


# Requirements and installation

- Linux (tested on Ubuntu 24.04)
- Java 11 (higher Java versions should also work)
- [ant](https://ant.apache.org/) and [maven](https://maven.apache.org/)
- [Compact Language Detector 2](https://github.com/CLD2Owners/cld2)


```bash
sudo apt install libcld2-0 libcld2-dev ant maven
```


# Compile Nutch and required projects

```bash
git clone git@github.com:crawler-commons/crawler-commons.git
cd crawler-commons/
mvn install
cd ..

git clone git@github.com:commoncrawl/language-detection-cld2.git
cd language-detection-cld2/
mvn install
cd ..

git clone https://github.com/commoncrawl/nutch.git nutch-cc
cd nutch-cc/
ant runtime
cd ..
```

# Configuration

Edit the configuration file in the `conf/nutch-site.xml`:
- It's required to configure at least the property `http.agent.name`
  in the file `conf/nutch-site.xml`.

Now there are two options:
1. If it's ensured that the script `crawl.sh` is kept in this project's root folder,
   continue with [Run crawl](#run-crawl). The configuration directory is added
   in front of the Java classpath, and the `nutch-site.xml` is picked from the
   first occurrence on the classpath.
2. If you plan to move scripts around or need more configurations, e.g., adapt the URL filter
   configuration files to your use case, then copy the file into `nutch-cc/conf/`:
   ```
   cp -p conf/nutch-site.xml nutch-cc/conf/
   ```
   After having done all configuration changes, Nutch needs to be recompiled because
   configuration files are contained in the job file (`runtime/local/apache-nutch-*.job`):
   ```
   cd nutch-cc/
   ant runtime
   cd ..
   ```

# Run crawl

```bash
echo -e "https://nutch.apache.org/\tnutch.score=1.0" >urls.txt

./crawl.sh crawl 3 urls.txt
```

