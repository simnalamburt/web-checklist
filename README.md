<img height=150px align=left src="http://resources.tugg.com/wp-content/uploads/2014/01/Icon-CampaignChecklist.png">

Web Checklist
========
> 서비스 릴리즈하기 전에 돌려보세요

<br>

-   [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights) - *Google*

    웹서버 속도를 테스트해줍니다. 최적화하는 방법도 같이 알려주기때문에
    유용합니다.

-   [Lighthouse](https://github.com/GoogleChrome/lighthouse) - *Google*

    웹페이지가 얼마나 모던 웹개발의 Best practice를 따라 만들어졌는지
    자동으로 검사해줍니다. 웹페이지 네트워크 설정, TLS 설정, 웹접근성 등의
    요소들을 종합적으로 평가해줍니다.

-   [Web Page Test](http://www.webpagetest.org/)

    PageSpeed와 비슷하게 웹서버 성능을 테스트해줍니다. 보다 여러 항목 (First
    Byte Time, Compress Image, ...) 에 대해 정성적인 결과를 내줍니다.

-   [SSL Server Test](https://www.ssllabs.com/ssltest) - *Qualys SSL Labs*<br>
    [SSL Configuration Generator](https://mozilla.github.io/server-side-tls/ssl-config-generator/) - *Mozilla*

    웹서버 HTTPS 세팅이 똑바로 되어있는지 확인합니다. 점수 규칙이 실용적이지
    못하기때문에, 만점을 받는것보단 **A+**를 내는것에 초점을 맞추셔야 합니다.

    그리고 테스트 결과와 별개로 [HPKP]도 활성화시키면 더욱 안전해집니다.

[HPKP]: https://en.wikipedia.org/wiki/HTTP_Public_Key_Pinning

-   [Markup Validation](https://validator.w3.org) - *W3C*

    서버가 HTML5 문법을 똑바로 지키는지 검사하고, 어떻게 고칠지 알려줍니다.

-   [Mobile Friendly Test](https://www.google.com/webmasters/tools/mobile-friendly) - *Google*

    페이지가 얼마나 모바일 프렌들리한지 알려줍니다.

-   [Favicon checker](https://realfavicongenerator.net/favicon_checker) - *Real Favicon Generator.net*

    페이지의 파비콘이 올바르게 들어가있는지 확인합니다. 브라우저 상단의 파비콘
    뿐만 아니라, 페이지로 바로가기를 만들었을때 어떻게 표시될지를 OS별로 모두
    검사해줍니다.

-   [Twitter Card Validator](https://cards-dev.twitter.com/validator)<br>
    [Facebook OpenGraph Debugger](https://developers.facebook.com/tools/debug/)

    내 웹페이지가 SNS에 공유되었을때 어떻게 표시될지 보여줍니다. 페이지에
    `<meta>` 태그를 추가하여 내용이 제대로 표시되도록 고쳐야합니다.

-   [Google Analytics](https://www.google.com/analytics/)<br>
    [Facebook Pixel](https://www.facebook.com/ads/manager/pixel/custom_audience_pixel/)

    방문자 통계 서비스입니다. 북유럽과 같은 일부 지역에서는 사전경고 없이
    방문자추적을 하면 불법이므로 유의해야합니다.

<br>

### Help Wanted!
쓰기 편한 온라인 웹서버 테스터 솔루션 아는것이 있다면
[추천해주세요!](https://github.com/simnalamburt/web-checklist/issues/new)

- [ ] 무료 부하테스트 서비스
- [ ] 그 외 기타 몸에 좋은 테스터들
