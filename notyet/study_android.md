# 안드로이드 공부

## 버튼 클릭시 반응

activity.java

```java
    public void onButton1Clicked(View v){
        Toast.makeText(getApplicationContext(), "시작버튼이 눌렸어요", Toast.LENGTH_LONG).show();
    }
```

```java
    public void onButton1Clicked(View v){
        Toast.makeText(getApplicationContext(), "시작버튼이 눌렸어요", Toast.LENGTH_SHORT).show();
    }
```

```java
    public void onButton2Clicked(View v){
        Intent myIntent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://m.naver.com"));
        startActivity(myIntent);
    }

    public void onButton3Clicked(View v){
        Intent myIntent = new Intent(Intent.ACTION_VIEW, Uri.parse("tel:010-0000-00000"));
        startActivity(myIntent);
    }
```

activity.xml

```xml
    <Button
        android:onClick="onButton1Clicked"/>

    <Button
        android:onClick="onButton2Clicked"/>

    <Button
        android:onClick="onButton3Clicked"/>
```

> 'Intent'는 안드로이드 스튜디오 플랫폼에게 원하는 것을 말할 때 전달하는 우편 개념이다.

---
