---
layout: post_cn
title: "java连接mongodb"
date: 2014-12-13 20:47:36
categories: JAVA
---

好久没有碰java了。最近尝试用java连接mongodb。

连接类：

    {% highlight java %}
    package net.xyjp.fanp.utils;

    import com.mongodb.DB;
    import com.mongodb.MongoClient;
    import java.net.UnknownHostException;
    import javax.annotation.PostConstruct;
    import javax.ejb.ConcurrencyManagement;
    import javax.ejb.ConcurrencyManagementType;
    import javax.ejb.Lock;
    import javax.ejb.LockType;
    import javax.ejb.Singleton;

    /**
     *
     * @author xiazhiyu
     */
    @Singleton
    @ConcurrencyManagement(ConcurrencyManagementType.CONTAINER)
    public class MongoClientProvider {
       
        private MongoClient mongoClient = null;
        
        final static String url = "localhost";
        
        final static int port = 27017;
        
        final static String db_name ="myerp";
        
        private DB db = null;
        
        @Lock(LockType.READ)
        public MongoClient getMongoClient(){    
            return mongoClient;
        }
        
        @Lock(LockType.READ)
        public DB getDb(){    
            return db;
        }
        
        @PostConstruct
        public void init() {

            try {
                mongoClient = new MongoClient(url, port);
                db = mongoClient.getDB(db_name);
            } catch (UnknownHostException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }        
        }   
        
    }
    {% endhighlight %}

调用示例：


    {% highlight java %}
    @Stateless
    @Path("user")
    public class UserFacadeREST extends AbstractFacade<User> {
      
      @EJB
      MongoClientProvider mongoClientProvider;

      DBCollection log = mongoClientProvider.getDb().getCollection("log");
      BasicDBObject logField = new BasicDBObject();
      logField.put("userId", rs.getId());
      logField.put("token", token);
      logField.put("time", System.currentTimeMillis());

    }
    {% endhighlight %}


