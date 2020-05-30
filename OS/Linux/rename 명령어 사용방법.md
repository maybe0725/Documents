
```
[Before]
  '001. 전미도-02-사랑하게 될 줄 알았어 - utoranking.com.mp3'

[CMD]
  rename -n 's/^\d\d\d. //' *

[After]
  '전미도-02-사랑하게 될 줄 알았어 - utoranking.com.mp3'
```

```
[Before]
  '전미도-02-사랑하게 될 줄 알았어 - utoranking.com.mp3'

[CMD]
  rename -n 's/ - utoranking.com//' *
  
[After]
  '전미도-02-사랑하게 될 줄 알았어.mp3'
```
