## Share

File path to Content Uri
```
  ContentValues content = new ContentValues();
  content.put(Video.VideoColumns.DATE_ADDED, System.currentTimeMillis() / 1000);
  content.put(Video.Media.MIME_TYPE, "video/mp4");
  content.put(Video.Media.DATA, "file path");
  ContentResolver resolver = context.getContentResolver();
  Uri uri = resolver.insert(Video.Media.EXTERNAL_CONTENT_URI, content);
```

File path to File Uri
```
  File file = new File("file path");
  Uri uri = Uri.fromFile(file);
```
