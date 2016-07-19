1.  克隆Git库

2.  下周mock工具[wiremock](http://wiki.uxin001.com/download/attachments/1376897/wiremock-standalone-2.1.7.jar?api=v2), 放在```api_mock```下（和```mappings```并列）

3.  运行mock工具```java -jar wiremock-standalone-2.1.7.jar --port xxxx```

    *   xxxx 是你配置的端口号，例如9999

4.  修改对应接口的配置文件，接口配置文件都在```mappings```下，根据需要修改```response```中的```jsonBody```

    ```json
    {
      "request" : {
        "urlPath" : "/room/create",
        "method" : "POST"
      },
      "response" : {
        "status" : 200,
        "jsonBody" : {
            "h": {
              "code" : 200,
              "msg" : "Success",
              "time" : "2016-07-11 20:37:12"
            },
            "b": {
                "roomId": 12345679000,
                "uid": 98765,
                "title": "不为人知的秘密",
                "price": 9.9,
                "liveStartTime": 1469642553116,
                "backPic": "http://uxin.com/pic/8329291.jpg",
                "backPicSmall": "http://uxin.com/pic/8329291_small.jpg",
                "introduce": "一个人偷偷的看"
            }
        }
      }
    }

    ```

5.  添加新的配置，以上面的配置为例，如果我有一个新的需求：请求接口```/room/create```，传入参数```name="nameless"```，要求返回```code: 123```&```roomId: 654321```，我们需要在对应的接口文件夹下添加新的配置文件（命名要有意义，例如room-create-123.json)，内容如下

    ```json
    {
      "request" : {
        "urlPath" : "/room/create",
        "method" : "POST",
        "bodyPatterns" : [ {
          "equalToJson" : "{\"name\": \"nameless\"}",
          "ignoreArrayOrder" : true,
          "ignoreExtraElements" : true
        } ]
      },
      "response" : {
        "status" : 200,
        "jsonBody" : {
            "h": {
              "code" : 123,
              "msg" : "Success",
              "time" : "2016-07-11 20:37:12"
            },
            "b": {
                "roomId": 654321
                "uid": 98765,
                "title": "不为人知的秘密",
                "price": 9.9,
                "liveStartTime": 1469642553116,
                "backPic": "http://uxin.com/pic/8329291.jpg",
                "backPicSmall": "http://uxin.com/pic/8329291_small.jpg",
                "introduce": "一个人偷偷的看"
            }
        }
      }
    }

    ```

6.  更新配置文件之后，需要重新运行```java -jar wiremock-standalone-2.1.7.jar --port xxxx```

