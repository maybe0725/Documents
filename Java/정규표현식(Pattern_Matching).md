정규표현식(Pattern Matching)
===========================

### 표현식
````````````````````````````````````````````````````````````````````````````
  ^  : 문자열의 시작
  $  : 문자열의 종료
  .  : 임의의 한 문자 (문자의 종류 가리지 않음). 단, \ 는 넣을 수 없음
  *  : 앞 문자가 없을 수도 무한정 많을 수도 있음
  +  : 앞 문자가 하나 이상
  ?  : 앞 문자가 없거나 하나있음
  [] : 문자의 집합이나 범위를 나타내며 두 문자 사이는 - 기호로 범위를 나타낸다. 
       []내에서 ^가 선행하여 존재하면 not 을 나타낸다
  {} : 횟수 또는 범위를 나타낸다.
  () : 소괄호 안의 문자를 하나의 문자로 인식 
  |  : 패턴 안에서 or 연산을 수행할 때 사용
  \s : 공백 문자
  \S : 공백 문자가 아닌 나머지 문자
  \w : 알파벳이나 숫자
  \W : 알파벳이나 숫자를 제외한 문자
  \d : 숫자 [0-9]와 동일
  \D : 숫자를 제외한 모든 문자
  \  : 정규표현식 역슬래시(\)는 확장 문자.
       역슬래시 다음에 일반 문자가 오면 특수문자로 취급하고,
       역슬래시 다음에 특수문자가 오면 그 문자 자체를 의미
(?i) : 앞 부분에 (?i) 라는 옵션을 넣어주면 대소문자를 구분하지 않음
````````````````````````````````````````````````````````````````````````````

### 자주 쓰이는 패턴
``````````````````````````````````````````````````````````````````````````````
1) 숫자만 : ^[0-9]*$
2) 영문자만 : ^[a-zA-Z]*$
3) 한글만 : ^[가-힣]*$
4) 영어 & 숫자만 : ^[a-zA-Z0-9]*$
5) E-Mail : ^[a-zA-Z0-9]+@[a-zA-Z0-9]+$
6) 휴대폰 : ^01(?:0|1|[6-9]) - (?:\d{3}|\d{4}) - \d{4}$
7) 일반전화 : ^\d{2,3} - \d{3,4} - \d{4}$
8) 주민등록번호 : \d{6} \- [1-4]\d{6}
9) IP 주소 : ([0-9]{1,3}) \. ([0-9]{1,3}) \. ([0-9]{1,3}) \. ([0-9]{1,3})
``````````````````````````````````````````````````````````````````````````````

### 예제1) 숫자만 허용
`````````````````````````````````````````````````````
import java.util.Scanner;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
 
public class OnlyInteger {
    
    public static void main(String[] args) {
        
        Pattern p = Pattern.compile("(^[0-9]*$)");
        
        int onlyNum;
        String inputVal;
        Scanner iStream = new Scanner(System.in);
        
        inputVal = iStream.nextLine();
        Matcher m = p.matcher(inputVal);
        
        if(m.find())
        {
            onlyNum = Integer.parseInt(inputVal);
            System.out.println(onlyNum);
        }
        else
        {
            System.out.println("숫자가 아닌데..?");
        }    
    }
}

[패턴분석]

(^[0-9]*$)

^ 으로 우선 패턴의 시작을 알립니다.
[0-9] 괄호사이에 두 숫자를 넣어 범위를 지정해줄 수 있습니다.
* 를 넣으면 글자 수를 상관하지 않고 검사합니다.
$ 으로 패턴의 종료를 알립니다.
`````````````````````````````````````````````````````

### 예제2) 영어만 허용 (Not Case Sensitive)
````````````````````````````````````````````````````````````
import java.util.regex.Pattern;
 
public class EngPattern {
    
    public static void main(String[] args) {
        
        String pattern = "^[a-zA-Z]*$";
        String input = "ABzzzDAWRAWR";
        
        
        
        boolean i = Pattern.matches(pattern, input);
        if(i==true)
        {
            System.out.println(input+"는 패턴에 일치함.");
        }
        else
        {
            System.out.println("패턴 일치하지 않음.");
        }
        
        
    }
    
}

[패턴분석]

^[a-zA-Z]*$

a-z 까지 그리고 A-Z 까지 즉, 알파벳은 모두 허용.
* 글자 수 상관하지 않음
$ : 끝

-> 알파벳이기만 하면 패턴에 맞음.
````````````````````````````````````````````````````````````

### 예제3) 파일 확장자 확인 1
````````````````````````````````````````````````````````````
package Pattern;
 
import java.util.regex.Pattern;
 
public class ExtendtionPattern {
    
    public static void main(String[] args) {
        
        
        String pattern = "^\\S+.(?i)(txt|pdf|hwp|xls)$";
        String input = "abc.txt";
        
        
        
        boolean i = Pattern.matches(pattern, input);
        if(i==true)
        {
            System.out.println(input+"는 패턴에 일치함.");
        }
        else
        {
            System.out.println("패턴 일치하지 않음.");
        }
    }
 
}

[패턴분석]

^\\S+.(?i)(txt|pdf|hwp|xls)$

^    : 시작
\    : \ 가 왔기 때문에 다음에 올 문자는 특수문자로 취급하고 , \다음 특수문자고 오면 그 자체로 취급.
\S   : 공백 아닌 문자
+.   : .이 반드시 한개는 와야한다.
(?i) : 대소문자 구별하지 않음.
(txt|pdf|hwp|xls) : txt 혹은 pdf 혹은 hwp 혹은 xls 만 허용. | 을 이용한 or 연산!
$ : 끝

-> 공백아닌 문자와 .이 반드시 와야하고 뒤에는 txt, pdf, hwp, xls 만 허용.
````````````````````````````````````````````````````````````

### 예제4) 파일 확장자 1, 2
```````````````````````````````````````````````````````````````````````
import java.util.regex.Pattern;
 
public class extensionPattern {
    
    public static void main(String[] args) {
    
        String pattern = "^\\S+.(?i)(txt|pdf|hwp|xls)$";
        String input = "Java.pdf";
        
        String pattern2 = "(.+?)((\\.tar)?\\.gz)$";
        String input2 = "library.tar.gz";
        
        
        vaildPattern(pattern, input);
        vaildPattern(pattern2, input2);    
    }
 
    
    public static void vaildPattern(String pattern, String input)
    {
        boolean i = Pattern.matches(pattern, input);
        
        if(i==true)
        {
            System.out.println(input+"는 패턴에 일치함.");
        }
        else
        {
            System.out.println("패턴 일치하지 않음.");
        }
    }
}
```````````````````````````````````````````````````````````````````````




