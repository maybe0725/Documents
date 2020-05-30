
```
[Before]
  '001. 전미도-02-사랑하게 될 줄 알았어 - maybe.com.mp3'

[CMD]
  rename -n 's/^\d\d\d. //' *

[After]
  '전미도-02-사랑하게 될 줄 알았어 - maybe.com.mp3'
```

```
[Before]
  '전미도-02-사랑하게 될 줄 알았어 - maybe.com.mp3'

[CMD]
  rename -n 's/ - maybe.com//' *
  
[After]
  '전미도-02-사랑하게 될 줄 알았어.mp3'
```
