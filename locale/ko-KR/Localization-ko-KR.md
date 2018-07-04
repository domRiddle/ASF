# 현지화

ASF는 모든 사람들이 ASF를 전세계의 모든 언어로 번역할 수 있도록 하는 Crowdin 서비스를 이용하고 있습니다. Crowdin이 어떻게 동작하는지 더 자세한 설명을 원하시면, **[Crowdin 소개](https://support.crowdin.com/crowdin-intro)**를 확인하시기 바랍니다.

현재 상황이 궁금하다면 **[ASF Crowdin 활동](https://crowdin.com/project/archisteamfarm/activity_stream)**에서 확인할 수 있습니다.

* * *

## 범위

우리 플랫폼은 메인 프로그램인 ASF와, 같이 제공하는 현지화 가능한 전체 콘텐츠의 현지화를 지원합니다. 즉, 웹 환경설정 생성기, IPC GUI 및 위키도 현지화에 포함됩니다. 모든 것이 편리한 Crowdin 인터페이스를 통해 번역가능합니다.

* * *

## 회원 가입

번역, 번역의 리뷰나 승인하는데 있어 ASF를 돕고 싶으시면 **[Crowdin 프로젝트 페이지](https://crowdin.com/project/archisteamfarm)**에서 회원으로 가입하시기 바랍니다. 회원등록은 쉽고 무료입니다. 로그인 후 할당받고 싶은 언어를 선택하고, ASF의 문자열로 와서 다른 커뮤니티 회원들이 ASF를 모든 유명한 언어로 번역하는 것을 도와주세요!

* * *

### 번역하기

선택한 언어의 문자열이 누락되어 있다면 번역을 시작하면 됩니다. 우리는 번역의 유연함을 위해 최선을 다하고 있습니다. 많은 문자열은 ASF가 실행되는 동안 제공하는 외부 변수를 포함하고 있으며, `{0}` 과 같이 괄호로 둘러쌓인 숫자로 표시됩니다. 이렇게 함으로써 엄격한 컨텍스트와 형식에 강제되지 않고, ASF가 제공하는 변수를 다른 언어와 번역에 맞게 위치를 옮기는 등 기본 ASF 문자열 형식을 변경할 수 있습니다. 이것은 히브리어 등 오른쪽에서 왼쪽으로 쓰는 언어에서 특히 중요합니다.

예를 들어, 아래와 같은 문자열이 있습니다.

> 농사지을 게임이 {0} 개 있습니다.

하지만 당신의 언어로는 다음 문장이 더 말이 될 수도 있습니다.

> 농사지을 게임의 수: {0}

또는,

> {0} 은 농사지을 게임의 갯수입니다.

이 유연성은 당신을 위해 특별히 제공되므로, 각 부분을 잘라서 번역하는 대신 ASF 문장을 당신의 언어에 더 알맞게 약간 말을 바꾸고 ASF에서 제공하는 숫자나 다른 정보를 당신이 한 번역에 맞는 장소로 옮길 수 있습니다. 이렇게 해서 전체적인 번역 품질이 개선됩니다.

* * *

### 리뷰하기

다른 사람이 이미 번역한 문자열을 선택했다면 투표를 할 수 있습니다. 투표를 통해 제일 처음 제안된 내용에 붙잡혀있지 않고 다양한 번역중 제일 좋은 것을 선택할 수 있습니다. 이렇게 해서 전체적인 번역의 품질이 훨씬 더 개선됩니다. 기 번역된 제안에 투표할 수도 있고, 동일한 절차를 따라 새로 번역을 하여 제안할 수도 있습니다. 결국 최종적인 문자열은 가장 많은 투표를 받은 제안이나 혹은 해당 언어에 대해 개인적으로 번역 승인을 받은 교정자의 선택으로 결정됩니다. (이경우에도 투표에 기초합니다)

**ASF에서 당신이 번역한 문자열을 보는데는 승인이 필요하지 않습니다.** 승인은 누군가 믿을만한 사람이 번역의 최종버전을 선택하듯이 단지 내용을 리뷰했다는 뜻입니다. 승인되지 않은, 커뮤니티에서 만든 번역이어도 최고라고 투표한다면 아무 문제 없습니다. 번역이 되어있기만 하면 모든게 문제 없습니다! 그리고 현재의 번역이 안좋다고 생각한다면 더 나은 번역에 투표하던지 직접 번역하여 제안할 수 있습니다!

* * *

### 교정하기

위에서 설명한 커뮤니티의 리뷰/투표 절차의 자유를 잠재적으로 없앤다하더라도, 일관되게 번역하는 것은 좋은 생각입니다. 이것은, 그렇게 나쁘지는 않은 부정확한 번역이 많은 반대투표를 받고, 더 나은 번역을 더이상 제안할 수 없는 경우가 있기 때문입니다.

Crowdin 혹은 우리가 신뢰하고 확인가능한 다른 현지화 플랫폼/서비스에서 기여한 내역이 있다면, 기여하고 계신 해당 언어의 교정접근권한을 드리고자합니다. 이를 통해 번역내용을 승인하고 일관되게 관리할 수 있습니다. 교정은 쉬운 작업이 아닙니다. 특히 ASF는 때때로 매우 "기술적"일 수 있으며 번역하기 매우 어려울 수 이있습니다. 하지만 완벽한 번역을 위해서 교정은 필요합니다. 따라서 해당 언어로 교정을 도와주실 수 있다면 **[우리에게 알려주십시오](https://crowdin.com/messages/create/13177432)**. 하지만 Crowdin의 ASF 현지화나 다른 프로젝트 등 검증할 수 있는 당신의 현지화 기여내역을 같이 보내주셔야 한다는 것을 명심하십시오. 우리가 개인적으로 알고 있으며 ASF를 해당언어로 가장 잘 현지화하기 위해 커뮤니티의 다른 사용자들과 협업할 수 있는 더 많은 고급사용자들에게 초벌 교정을 선택권한을 줄 수 있습니다.

일반 규칙은 교정에도 적용됩니다. 서두르지 말고, 이용자들의 의견을 듣고, 프로젝트 매니저로써 작업하고, 이슈를 해결하고, 상황을 나쁘게 만들지 말고 개선하여야 합니다.

* * *

### 이슈

어떻게 번역할지 모르거나, 승인된 번역이 틀렸거나, 더욱 정확한 문맥이 필요하는 등 특정 번역에 문제가 있는 경우, [X] 이슈로 표시하여 해당 문자열에 댓글로 달아주시기 바랍니다.

**기술적/개발 설명이나 관리자 작업이 필요하지 않은 경우 이슈마크를 사용하지 마십시오.** 해당 문자열에 대한 토론을 위해 댓글을 자유롭게 이용할 수 있습니다. 하지만 이슈는 기술적 설명이나 관리자의 수정이 필요할 때에만 사용하여야 합니다. 또한 이는 당신이 번역중인 언어를 구사할 수 없는 누군가의 개입이 필연적이므로, 이슈 댓글 작성시 영어로 해주시기 바랍니다. 그래야 우리가 이슈가 무엇인지를 이해할 수 있습니다.

현재 4가지 종류의 이슈를 지원합니다:

* 일반 질문 - 아래 이슈에 해당하지 않는 모든 경우를 말합니다. 일반적으로 이 종류는 **피해주시기 바랍니다**. 만약 문제가 아래 종류에 해당사항이 없다면 번역 이슈가 **아닐 가능성이 높습니다**. 모든 경우의 수를 대비해서 만들어 둔 상태입니다.
* 현재 번역이 잘못됨 - 이미 교정자에 의해 승인된 번역이지만 오타가 있거나 번역을 개선할 유효한 제안이 있는 경우 등 **틀렸다고** 판단될때에만 사용하시기 바랍니다. 또한, 커뮤니티나 투표로 정해진 번역에는 절대 사용하지 마십시오. 이 경우 해당 번역자와 연락하여 수정을 요청하거나, 혹은 리뷰하기 항목에서 설명한 것 처럼 더 나은 번역에 투표하십시오.
* 문맥 정보 부족 - 현재 번역중인 내용이 ASF의 어느 부분인지, 해당 문자열의 문맥 혹은 의도가 무엇인지 확실하지 않을때 사용하시기 바랍니다. 이 종류는 ASF 개발쪽에서만 사용하여야 합니다. 즉 해당 문자열을 어떻게 번역할지 확실치 않아서 기술적 지원이 필요하다는 뜻입니다.
* 원본 문자열 오류 - 영어 원본 문자열이 틀렸다고 생각하는 경우에만 사용하십시오. 상당히 드문 경우이긴 합니다만, 개발자 본인도 영어 원어민이 아니므로 원본을 어떻게 개선할지 아이디어가 있다면 자유롭게 이용하시기 바랍니다.

* * *

### 번역 진행도

모든 언어는 번역과 교정, 두 개의 완료단계가 있습니다.

번역 진행도가 100%에 도달하면 해당 언어로 **번역되었다**고 판단됩니다. 이 시점에서 ASF에서 사용된 모든 현지화 가능한 문자열은 아주 적절한 의미를 갖게됩니다. 그러나, 개선의 여지가 없다는 뜻은 아닙니다. 커뮤니티의 투표는 항상 가능하고, 당신은 기 번역된 부분에 대해 투표하거나 더 나은 번역을 제안할 수 있습니다. 개발과정에서 기존의 문자열을 변경하거나 새롭게 추가하게되면 완전히 번역된 언어도 100% 아래로 떨어질 수 있다는 점을 양지하시기 바랍니다. 이런 일이 일어났을때 전자우편을 받고 싶다면 적절한 Crowdin 알림을 설정할 수 있있습니다.

선택된 언어에는 적절한 교정자가 있을 수 있으며, 이들은 번역을 검증하고 최종 버전을 승인합니다. 이는 번역이 이루어진 후 최종 단계이며, 현지화를 더욱 개선할 수 있게 해줍니다.

ASF는 해당 언어를 **가능한 한 빨리** 추가할 것입니다. 즉, 승인을 받거나 심지어 100% 번역되지 않아도 된다는 뜻입니다. 드문 일이지만 선택된 교정자가 다르게 결정하지 않는한, 항상 투표에서 가장 인기있는 문자열이 실제 문자열로 사용될 것입니다. 따라서, 번역이 Crowdin에 제출되자마자 당신의 노력이 바로 다음번 ASF 릴리즈에 포함된 것을 볼 수 있을것입니다. 현지화 업데이트는 ASF의 새버전을 릴리즈 하려는 그 순간에 합쳐집니다.

* * *

## 언어가 없는 경우

ASF 프로젝트는 세계적으로 사용되는 상위 30개의 언어에 대해서만 번역이 가능합니다. 만약 다른 언어, 혹은 이미 있는 언어의 지역 방언을 추가하고 싶다면 **[우리에게 알려주시기 바랍니다](https://crowdin.com/messages/create/13177432)**. 가능한 한 빨리 추가하겠습니다. 아무도 번역하는 사람이 없는 몇백개의 서로 다른 언어를 번역가능 상태로만 두고 싶지 않기 때문에 일정 숫자로 제한하였습니다. 목록에 없는 언어로 번역하고 싶다면 바로 알려주십시오. 사실 언어를 추가하는 것은 매우 쉽습니다.

ASF 프로젝트의 번역 가능한 전체 언어 목록은 **[여기를 참고하십시오](https://support.crowdin.com/api/language-codes)**.

* * *

## 위키

Crowdin 플랫폼에서는 심지어 위키 자체를 현지화 할 수 있습니다. Crowdin은 매우 강력한 도구로, 전체 ASF 설명서를 당신의 언어로 만들수 있으며 ASF 현지화의 가장 최근 이슈를 효과적으로 해결하였습니다. ASF 프로그램과 각 부분의 번역을 통해 완전히 현지화됩니다.

위키는 원래 문장에 너무 얽매일 필요가 없는 온라인 도움말이므로 이점에서 약간 특별합니다. 즉, 원본 문자열과 사용된 단어, 실제 문장부호에 집착할 필요 없이 가능한한 자연스러운 언어로 원래의 뜻과 도움말을 전달하기 원한다는 것입니다. 문장에 포함된 전체적 방향성과 도움말을 유지하고 있다면, 문자열을 훨씬 더 자연스러운 당신의 언어로 새로 쓰는 것을 두려워하지 마십시오.

* * *

### Global links

Our crowdin platform also allows you to adapt the original text in order to make it point to new (localized) locations.

ASF includes links on almost every page for easier navigation, as well as sidebar on the right. The awesome fact is that you can edit all of that, "fixing" links to point to proper localized pages for your language. It requires to be a bit careful doing that, but it's possible.

For example, ASF **[home page](https://github.com/JustArchi/ArchiSteamFarm/wiki/Home)** includes a text such as:

> 처음 오셨다면 **[설치하기](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-ko-KR)** 가이드부터 시작하는 것을 추천합니다.

Which is originally written as:

```markdown
If you're a new user, we recommend starting with **[setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** guide.
```

On the crowdin, first thing you should do is going to your editor settings and ensuring that HTML tags are set to "Show" for you. This is very important if you decide to localize the wiki.

* * *

![Crowdin](https://i.imgur.com/YqAxiZ4.png)

* * *

Now, during translating on the crowdin, depending on formatting, you'll see ASF links in the text either as:

* HTML 태그와 번역할 문자자열이 함께 있는 경우(대부분의 문자열에 해당하며, 문장의 일부분이 링크)
* 문자열 자체는 따로 있고 링크는 `Hidden texts` -> `Link addresses`에 포함된 경우(드물지만 전체 문자열이 링크임. 특히 사이드바)

In our example above, it's the first case (since only "setting up" is a link), so in crowdin we'll see it as:

* * *

![Crowdin 2](https://i.imgur.com/Li5RzS3.png)

* * *

Regardless of case, firstly you click ALT+C (or copy source button) and translate it as usual, leaving entire HTML (if present) in-tact. This would be example of translation for Polish language:

* * *

![Crowdin 3](https://i.imgur.com/NpKwfka.png)

* * *

Now, if the link is a generic link that points outside of the wiki (e.g. to latest ASF release), you can leave it as it is since you don't want to edit it. You can save it and move forward.

However, if the link **does** point further inside the wiki, like the one above, you can actually correct it to point to new (localized) location. You do this by carefully appending `-locale` to target URL in `<a>` tag, like below:

* * *

![Crowdin 4](https://i.imgur.com/TL8uwmb.png)

* * *

Be extremely careful about this, and ensure that your URL indeed exists, since if you make a mistake, that link will stop functioning. If you succeeded, you now have a fully functional translation with link pointing to translated (in our case `Setting-up-pl-PL`) page.

Doing the steps above will properly translate our HTML back to markdown:

```markdown
Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika po konfiguracji](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-pl-PL)**.
```

And finally into wiki text:

> Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika po konfiguracji](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-pl-PL)**.

When no HTML is present (second case), this is even easier since you can just go to `Hidden texts` -> `Link addresses`.

* * *

![Crowdin 5](https://i.imgur.com/ZmgavCM.png)

* * *

From there you can easily correct the link to point to new location, without even bothering with HTML at all:

* * *

![Crowdin 6](https://i.imgur.com/maG7kSm.png)

* * *

### Local links

Across the wiki you will also find local links that point to particular section of the document. Those links start with `#` character.

Now those are special cases, since those links are based on names of the sections of current document. While for URLs we have general convention of adding `-locale` to the URL, and it works everywhere, section names will be translated by you and other people, so you need to ensure that they point to proper location.

For example you can find `#introduction` link in our **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#introduction)** section:

* * *

![Crowdin 7](https://i.imgur.com/EEHSPtK.png)

* * *

Since we're going to translate "Introduction" word into "Wprowadzenie" for our Polish language, we'll need to correct this link since it'll stop functioning the moment we do this.

* * *

![Crowdin 8](https://i.imgur.com/JMJegO7.png)

* * *

This way our local link will keep working, since it'll now point to name of the section that we're using. You can correct links inside HTML tags in exactly the same way.

* * *

### Code blocks

Be extremely careful when you translate sentences with `<code></code>` blocks inside. Code block indicates fixed ASF code names or terms that should not be translated. 예를 들면 다음과 같습니다:

    This is especially useful if you have a lot of keys to redeem and you're guaranteed to hit <code>RateLimited</code> status before you're done with your entire batch.
    

As you can see, `RateLimited` word here is inside a code block and indicates internal ASF code status - this should not be translated. Likewise, you shouldn't translate other code blocks, such as names of config properties (e.g. `TradingPreferences`), enum members (e.g. `Stable` and `Experimental` options of `UpdateChannel`) and likewise.

If you believe that something inappropriate is included in a code block, or that there is a text that is not in a code block but should be inside it, feel free to ask on our crowdin by creating appropriate **[issue](#issues)**.

* * *

ASF를 전세계에서 사용되는 모든 언어로 번역하는데 도와주셔서 감사합니다!