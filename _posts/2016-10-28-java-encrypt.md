---
permalink: /java-encrypt
title: "개인정보보호법과 JAVA를 이용한 암호화 구현(SHA256, AES256)"
excerpt: "개인정보보호법의 내용을 이해하고 단방향 양방향 알고리즘의 대표적인 암호화 알고리즘인 AES256과 SHA256을 JAVA로 어떻게 구현할 수 있는지 알아봅니다."
header:
  #image: /assets/images/common/teaser.jpg
  og_image: /assets/images/content/2016/10/28/java-encrypt/teaser.png
  overlay_image: /assets/images/content/2016/10/28/java-encrypt/teaser.png
  overlay_filter: 0.5
  cta_url: "https://github.com/binarythink/encryption"
  cta_label: "Go Repository"
categories:
  - java
tags: 
  - 암호화
  - 개인정보보호
  - JCE
  - AES256
  - SHA256
toc : true
last_modified_at: 2017-12-05T16:12:05
---

## 개요

온라인의 대규모 개인정보 유출사례가 많아지면서 한국에서도 '개인정보보호법'[^1]과 '정보통신망법'[^2]을 통해 수집·유출·오용·남용으로부터 사생활의 비밀 등을 보호하고 개인정보처리에 관한 규정을 만들었습니다. 최근 프로그램을 만들거나 웹사이트를 구성할때 정보통신망법을 따라 최소한의 개인정보만 사용자 동의를 얻어 저장하고, 혹시 모를 위협에 대비해 암호화를 하고 있습니다.

[^1]: 개인정보보호법 [전문 바로가기](http://www.law.go.kr/%EB%B2%95%EB%A0%B9/%EA%B0%9C%EC%9D%B8%EC%A0%95%EB%B3%B4%20%EB%B3%B4%ED%98%B8%EB%B2%95)
[^2]: 정보통신망법 [전문 바로가기](http://www.law.go.kr/%EB%B2%95%EB%A0%B9/%EC%A0%95%EB%B3%B4%ED%86%B5%EC%8B%A0%EB%A7%9D%EC%9D%B4%EC%9A%A9%EC%B4%89%EC%A7%84%EB%B0%8F%EC%A0%95%EB%B3%B4%EB%B3%B4%ED%98%B8%EB%93%B1%EC%97%90%EA%B4%80%ED%95%9C%EB%B2%95%EB%A5%A0)

## 개인정보의 개념

'개인정보보호법'에는 개인정보의 정의를 다음과 같이 내리고 있습니다.

> "개인정보"란 살아 있는 개인에 관한 정보로서 성명, 주민등록번호 및 영상 등을 통하여 개인을 알아볼 수 있는 정보(해당 정보만으로는 특정 개인을 알아볼 수 없더라도 다른 정보와 쉽게 결합하여 알아볼 수 있는 것을 포함한다)를 말한다.


## 관련용어

암호(Cryptography)
: 해독 불가능한 형태로 변환하거나 또는 암호화된 메시지를 해독 가능한 형태로 변환하는 기술

평문(Plaintext) 
: 해독 가능한 형태의 메시지

암호문(Ciphertext)
: 해독 불가능한 형태의 메시지

암호화(Encryption)
: 평문을 암호문으로 변환하는 과정

복호화(Decryption)
: 암호문을 평문으로 변환하는 과정

대칭키 암호(또는 비밀키 암호)
: 암호화키와 복호화키가 같은 암호

비대칭키 암호(또는 공개키 암호)
: 암호화키와 복호화키가 다른 암호


## 단방향 VS 양방향 알고리즘

개인정보보호의 방법은 법령을 읽어보면 다양한 분야에서 신경써야 하지만 개발자가 가장많이 마주하게 되는 것이 개인정보를 암호화하여 저장하고 확인하는 방법입니다. 암호화 알고리즘의 특징중의 하나로 단방향(일방향) 암호화 알고리즘과 양방향 암호화 알고리즘이 있는데 각 각의 특징과 대표적인 암호화 방법을 알아보겠습니다.


### 단방향(일방향) 암호화 알고리즘

단방향 암호화는 평문을 암호화했을때 다시 평문으로 되돌리는 것(복호화)을 할 수 없는 암호화 입니다. 많이 사용하고 있는 알고리즘은 SHA-256 암호화입니다. ~~이 알고리즘은 주로 사용자의 패스워드 에 사용하는데, 패스워드의 경우 복호화해서 식별할 필요가 없기 때문입니다. e-mail 이나 연락처 이름 등은 서비스상에서 안내등을 위해서 원래의 값이 무엇인지 평문으로 만들 필요가 있지만 패스워드는 평문으로 만들 필요가 없는 정보로 분류해서 SHA-256 을 주로 사용합니다.~~[^password]

[^password]: 최근 패스워드를 SHA-256을 사용하게 되는 경우 발생할 수 있는 문제점에 대한 글을 확인해서 공유합니다. Naver D2 블로그 '안전한 패스워드 저장'의 단방향 해시 함수의 문제점 부분에서 패스워드로 SHA-256 암호화를 사용하면 어떤 문제가 있는지 자세히 확인 하실 수 있습니다. [바로가기](http://d2.naver.com/helloworld/318732)


### 양방향 알고리즘

양방향 암호화 알고리즘은 평문에서 암호문으로, 암호문에서 평문으로 변환하는 암호화와 복호화가 이루어지는 알고리즘 입니다. 많이 사용하는 알고리즘은 AES-256 입니다. 주로 이름, 주소, 연락처 등 복호화 하는데 필요한 정보를 이 알고리즘을 이용해서 암호문으로 관리합니다.



## Java 를 이용한 구현
### 개발전 준비단계
기본으로 제공하는 JDK를 설치하면 암호 알고리즘을 만들 수 있는 API가 제공되지만, AES-128 보다 한단계 더 높은 단계인 AES-256을 구현하기 위해서는 별도의 라이브러리 확장 파일이 필요합니다.
JDK 다운로드 페이지에 가면 아래처럼 JCE 를 다운받을 수 있습니다.

자신의 JRE 버전에 맞는 해당 파일을 다운로드 받아서 JRE 폴더의 `/lib/security/` 위치에 `local_policy.jar` 파일과 `US_export_policy.jar` 파일을 복사합니다.


### SHA256
복호화가 없으므로 별도의 키가 없습니다. 또 키가 없으므로 SHA256 알고리즘을 돌려서는 항상 같은 결과가 떨어집니다. 1과 같은 단순한 값을 sha256으로 암호화했을 경우 항상 같은 값이 나오기 때문에 비밀번호 입력을 적당한 길이와 복잡성을 가지도록 유도하여 패스워드를 유추하는데 어렵도록 해야 합니다.

```java
import java.security.MessageDigest;

public class Sha256 {
    public static String encrypt(String planText) {
        try{
            MessageDigest md = MessageDigest.getInstance("SHA-256");
            md.update(planText.getBytes());
            byte byteData[] = md.digest();

            StringBuffer sb = new StringBuffer();
            for (int i = 0; i < byteData.length; i++) {
                sb.append(Integer.toString((byteData[i] & 0xff) + 0x100, 16).substring(1));
            }

            StringBuffer hexString = new StringBuffer();
            for (int i=0;i<byteData.length;i++) {
                String hex=Integer.toHexString(0xff & byteData[i]);
                if(hex.length()==1){
                    hexString.append('0');
                }
                hexString.append(hex);
            }

            return hexString.toString();
        }catch(Exception e){
            e.printStackTrace();
            throw new RuntimeException();
        }
    }
}
```

### AES-256
실제로 프로젝트에서 사용하기 위해 만들었던 코드로 `org.apache.commons.codec` 라이브러리에 의존성을 가지고 있다. `AES256Util` 객체를 생성할때에는 암호화/복호화에 사용될 키를 입력받는다. 스프링 빈컨테이너에 키를 넣어 생성해서 사용하기 위해서 만들었으나 쉽게 생성자를 수정하고 맴버변수를 등록하도록 수정하여 사용할 수도 있다, 단 키의 길이가 16자리 이하일 경우 오류가 발생한다. 개발전 준비단계에 언급한 `local_policy.jar` 파일과 `US_export_policy.jar` 추가 다운로드한 라이브러리가 없을 경우에는 16자리의 키를 입력하더라고 `java.security.InvalidKeyException: Illegal key size`이 발생한다.

```java
import java.io.UnsupportedEncodingException;
import java.security.GeneralSecurityException;
import java.security.Key;
import java.security.NoSuchAlgorithmException;

import javax.crypto.Cipher;
import javax.crypto.spec.IvParameterSpec;
import javax.crypto.spec.SecretKeySpec;

import org.apache.commons.codec.binary.Base64;

/**
 * 양방향 암호화 알고리즘인 AES256 암호화를 지원하는 클래스
 */
public class AES256Util {
    private String iv;
    private Key keySpec;

    /**
     * 16자리의 키값을 입력하여 객체를 생성한다.
     * @param key 암/복호화를 위한 키값
     * @throws UnsupportedEncodingException 키값의 길이가 16이하일 경우 발생
     */
    public AES256Util(String key) throws UnsupportedEncodingException {
        this.iv = key.substring(0, 16);
        byte[] keyBytes = new byte[16];
        byte[] b = key.getBytes("UTF-8");
        int len = b.length;
        if(len > keyBytes.length){
            len = keyBytes.length;
        }
        System.arraycopy(b, 0, keyBytes, 0, len);
        SecretKeySpec keySpec = new SecretKeySpec(keyBytes, "AES");

        this.keySpec = keySpec;
    }

    /**
     * AES256 으로 암호화 한다.
     * @param str 암호화할 문자열
     * @return
     * @throws NoSuchAlgorithmException
     * @throws GeneralSecurityException
     * @throws UnsupportedEncodingException
     */
    public String encrypt(String str) throws NoSuchAlgorithmException, GeneralSecurityException, UnsupportedEncodingException{
        Cipher c = Cipher.getInstance("AES/CBC/PKCS5Padding");
        c.init(Cipher.ENCRYPT_MODE, keySpec, new IvParameterSpec(iv.getBytes()));
        byte[] encrypted = c.doFinal(str.getBytes("UTF-8"));
        String enStr = new String(Base64.encodeBase64(encrypted));
        return enStr;
    }

    /**
     * AES256으로 암호화된 txt 를 복호화한다.
     * @param str 복호화할 문자열
     * @return
     * @throws NoSuchAlgorithmException
     * @throws GeneralSecurityException
     * @throws UnsupportedEncodingException
     */
    public String decrypt(String str) throws NoSuchAlgorithmException, GeneralSecurityException, UnsupportedEncodingException {
        Cipher c = Cipher.getInstance("AES/CBC/PKCS5Padding");
        c.init(Cipher.DECRYPT_MODE, keySpec, new IvParameterSpec(iv.getBytes()));
        byte[] byteStr = Base64.decodeBase64(str.getBytes());
        return new String(c.doFinal(byteStr), "UTF-8");
    }
}
```

### 그 외의 알고리즘
그 외에 포스팅을 위해 검색해보니 [한국인터넷진흥원 암호화이용활성화](http://seed.kisa.or.kr/) 사이트에서 자체적으로 개발한 암호 기능[^3]들을 다운로드 받을 수 있었다. 자세한 내용은 홈페이지를 이용하고 자료실에 각 언어로 작성된 파일을 다운로드 받을 수 있으므로 이들을 활용해보는 것도 좋은 방법이다.

[^3]: KISA 암호이용활성화 : [http://seed.kisa.or.kr/](http://seed.kisa.or.kr/)