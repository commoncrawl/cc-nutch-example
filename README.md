Example Usage of Common Crawl's Fork of Apache Nutch to Crawl and Write WARC files
==================================================================================



# Requirements and installation

- Linux (tested on Ubuntu 18.04)
- Java 8 (higher Java versions should also work)
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

# Run crawl

```bash
echo -e "https://nutch.apache.org/\tnutch.score=1.0" >urls.txt

./crawl.sh crawl 3 urls.txt
```

