+++
metaAlignment = "center"
thumbnailImage = ""
tags = [
  "software","retrofit","multipart","file upload"
]
autoThumbnailImage = false
desciption = ""
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
title = "Retrofit Multipart File Upload"
categories = [
  "yazilim",
  "android",
]
keywords = [
  "yazilim",
  "sofware",
  "retrofit",
  "multipart",
  "file upload"
]
thumbnailImagePosition = "top"
date = "2017-01-11T01:50:51+03:00"

+++

# Retrofit Multipart File Upload

```java
    @DebugLog
    public BelgeResponse BelgeSync(BelgeRequest request, String apiVersion, String appVersion) {
        try {

            Map<String, RequestBody> map = new HashMap<>();
            String belgeDirPath = SuperHelper.getInternalIztopBelgeDir(mContext, request.getGonderiNo());
            File belgeDir = new File(belgeDirPath);
            boolean isDir = belgeDir.isDirectory();
            if (isDir) {
                File[] files = belgeDir.listFiles();
                for (File file : files) {
                    RequestBody requestBody = RequestBody.create(MediaType.parse("image/jpg"), file);
                    map.put("file\"; filename=\"" + file.getName(), requestBody);
                }
            }

            //RequestBody requestBodyGonderiNo = RequestBody.create(MediaType.parse("text/plain"), request.getGonderiNo());
            //RequestBody requestBodyMusId = RequestBody.create(MediaType.parse("text/plain"), String.valueOf(request.getMusId()));

            RestClient restClient = RestClient.getInstance();
            Call<BelgeResponse> responseCall = restClient.getApiService().Belge(
                    apiVersion,
                    appVersion,
                    request.getGonderiNo(),
                    String.valueOf(request.getMusId()),
                    map);
            BelgeResponse resp = responseCall.execute().body();
            return resp;
        } catch (Exception ex) {
            SuperHelper.CrashlyticsLog(ex);
            return null;
        }
    }
```    
