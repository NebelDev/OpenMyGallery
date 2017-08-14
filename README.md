# OpenMyGallery

This sample app shows you how you can pick a photo from your Android gallery and show it on your app. An example of using this app is make the user personalize his image profile.

### Explanation

```java
Intent intent = new Intent(Intent.ACTION_GET_CONTENT, android.provider.MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
intent.setType("image/*");
startActivityForResult(intent, PICK_PHOTO_FOR_AVATAR);
```

The intent we created is asking to the content provider to retrieve the image. The result of this int request are delivered in the Android function that we will override *onActivityResult*.

```java
 @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_PHOTO_FOR_AVATAR && resultCode == Activity.RESULT_OK&& null != data) {
            Uri selectedImage = data.getData();
            String[] filePathColumn = { MediaStore.Images.Media.DATA };

            Cursor cursor = getContentResolver().query(selectedImage,
                    filePathColumn, null, null, null);
            cursor.moveToFirst();

            int columnIndex = cursor.getColumnIndex(filePathColumn[0]);
            String picturePath = cursor.getString(columnIndex);
            cursor.close();
            if(picturePath == null)
                img.setImageURI(selectedImage);
            else
                img.setImageBitmap(BitmapFactory.decodeFile(picturePath));

        }
```
As you can see I query the content provider and I get back a cursor. Once I have my cursor it's easy to resolve our image and then set the ImageView with our image gallery just selected.
