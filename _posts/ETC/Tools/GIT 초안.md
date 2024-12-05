


나의 말:
현재 github 블로그를 구글서치에 등록하려는데, 아래 설명을 좀 더 자세히 설명해 줄래?

▪ Goolge Search Console 연결
구글에 내 게시물이 보이게 하려면 search console과 연결이 필요 https://search.google.com/search-console/about에서 접속하여 도메인 등록

도메인 등록 시 구글에서 제공하는 google~~.html 파일 루트 디렉토리에 업로드
jekyll-sitemap 플러그인 설치 (구글링 추천)
sudo gem install jekyll-sitemap
_config.yml 파일에 plugins에 jekyll-sitemap 없으면 추가
# Plugins (previously gems:)
plugins:
  - jekyll-sitemap
루트 디렉토리에 robots.txt 생성
User-agent: *
Allow: /

Sitemap: https://github-account.github.io/sitemap.xml
ChatGPT의 말:
ChatGPT
GitHub 블로그를 Google Search Console에 등록하여 검색엔진에 잘 노출되도록 하는 방법을 단계별로 자세히 설명해 줄게요.

1. Google Search Console에 연결하기
Google Search Console은 구글 검색 결과에 블로그 콘텐츠를 등록하고 관리할 수 있는 도구입니다. 블로그를 구글에 노출시키려면 이 콘솔에 블로그 도메인을 추가해야 합니다.
Google Search Console에 접속하여 “시작하기” 버튼을 클릭하세요.
2. 도메인 등록하기
Search Console에서는 두 가지 등록 옵션을 제공합니다. 도메인과 URL 접두어 방식이 있는데, GitHub Pages 블로그의 경우 URL 접두어 옵션을 선택하는 것이 간편합니다.
GitHub Pages 블로그는 https://your-github-username.github.io/ 형식의 URL을 가지므로 URL 접두어를 선택하고 블로그 URL을 입력하세요.
3. 사이트 소유권 확인하기
도메인 등록 시 Google은 블로그 소유권 확인을 요구합니다. 일반적으로 제공되는 방법은 다음과 같습니다.
HTML 파일 업로드: Google에서 제공하는 google12345.html 형식의 파일을 루트 디렉토리에 업로드합니다.
이 파일은 GitHub 블로그의 루트 디렉토리에 넣어야 하기 때문에 GitHub Pages에서는 _config.yml 파일을 설정하여 HTML 파일을 배포할 수 있습니다.
제공된 HTML 파일을 블로그 루트 디렉토리에 업로드한 후, https://your-github-username.github.io/google12345.html에 접속하여 올바르게 업로드되었는지 확인합니다.
HTML 메타 태그 삽입: Google에서 제공하는 메타 태그를 블로그의 <head> 섹션에 추가할 수도 있습니다.
메타 태그 방식을 사용하려면 _includes 폴더에 head.html 파일을 만들어 메타 태그를 추가하고, _layouts 폴더의 default.html 파일에 {% include head.html %} 구문을 삽입합니다.
4. Jekyll Sitemap 플러그인 설치
Sitemap은 검색 엔진이 블로그 구조를 쉽게 파악할 수 있도록 돕는 일종의 "사이트 지도"입니다. jekyll-sitemap 플러그인은 블로그 게시물과 페이지를 모아 자동으로 sitemap.xml 파일을 생성해 줍니다.
터미널에서 jekyll-sitemap 플러그인을 설치합니다:
bash
코드 복사
sudo gem install jekyll-sitemap
설치가 완료되면, 블로그의 _config.yml 파일을 열고 다음과 같이 plugins 목록에 jekyll-sitemap을 추가하세요:
yaml
코드 복사
plugins:
  - jekyll-sitemap
5. robots.txt 파일 생성
robots.txt 파일은 검색 엔진이 사이트 내 어떤 부분을 크롤링할 수 있는지 지정하는 파일입니다.
프로젝트 루트 디렉토리에 robots.txt 파일을 생성하고 아래 내용을 추가하세요:
plaintext
코드 복사
User-agent: *
Allow: /

Sitemap: https://your-github-username.github.io/sitemap.xml
이 파일은 모든 검색 엔진이 사이트의 모든 페이지를 크롤링하도록 허용하며, sitemap.xml 파일 위치를 명시합니다.
6. Google Search Console에서 사이트맵 제출
Search Console에서 사이트맵 메뉴로 이동하여 https://your-github-username.github.io/sitemap.xml을 입력하고 제출합니다.
성공적으로 제출되면 Google이 블로그의 사이트맵을 크롤링하고, 검색 엔진 결과에 노출하기 시작합니다.
이 과정을 거치면 블로그가 Google에 잘 노출될 수 있습니다. 설정 후에도 검색 결과에 노출되기까지는 다소 시간이 걸릴 수 있으니, 꾸준히 업데이트를 진행하면서 지켜보세요.






나의 말:
터미널에서 jekyll-sitemap 플러그인을 설치한다고 했는데, 내 로컬 컴퓨터 터미널을 말하는거야?
ChatGPT의 말:
ChatGPT
네, 맞습니다. 여기서 터미널은 로컬 컴퓨터의 터미널을 의미합니다.

jekyll-sitemap 플러그인은 로컬에서 블로그를 빌드할 때 자동으로 사이트맵을 생성해주는 역할을 하기 때문에, 로컬 환경에 설치해야 합니다. 로컬에서 설치하고 Jekyll로 블로그를 빌드할 때, 생성된 파일을 GitHub 저장소에 푸시하면 GitHub Pages에서 sitemap.xml 파일을 포함한 최신 블로그 사이트를 호스팅하게 됩니다.

설치 및 빌드 순서 요약
터미널에서 jekyll-sitemap 설치:
bash
코드 복사
sudo gem install jekyll-sitemap
_config.yml 파일 수정:
_config.yml 파일에 jekyll-sitemap을 plugins 항목에 추가합니다.
로컬에서 Jekyll 빌드 후 사이트 테스트:
사이트맵이 제대로 생성되는지 확인하려면 로컬에서 Jekyll로 사이트를 빌드하고, _site/sitemap.xml 파일이 생성되었는지 확인합니다.
GitHub에 푸시:
모든 설정이 완료되면 변경 사항을 GitHub에 푸시하세요.
이후 GitHub Pages에서 자동으로 사이트맵 파일을 포함한 최신 블로그 사이트가 반영됩니다.






나의 말:
PS C:\Users\khju\Documents\Project_TBD\JavierJu.github.io> sudo gem install jekyll-sitemap
sudo : 'sudo' 용어가 cmdlet, 함수, 스크립트 파일 또는 실행할 수 있는 프로그램 이름으로 인식되지 않습니다. 이름이 정확한지 확인하고 경로가 
포함된 경우 경로가 올바른지 검증한 다음 다시 시도하십시오.
위치 줄:1 문자:1
+ sudo gem install jekyll-sitemap
+ ~~~~
    + CategoryInfo          : ObjectNotFound: (sudo:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
ChatGPT의 말:
ChatGPT
Windows 환경에서는 sudo 명령어가 지원되지 않아 생기는 오류입니다. Windows에서는 관리자 권한으로 터미널을 실행한 후 sudo 없이 명령어를 입력해야 합니다.

해결 방법
관리자 권한으로 PowerShell 열기

시작 메뉴에서 PowerShell을 검색한 다음, 관리자 권한으로 실행을 선택합니다.
gem 명령어 입력

관리자 권한의 PowerShell에서 아래 명령어를 입력해 jekyll-sitemap을 설치합니다:
powershell
코드 복사
gem install jekyll-sitemap
이후 설치가 정상적으로 완료되면 _config.yml 파일을 수정하여 jekyll-sitemap을 추가한 다음, 로컬에서 블로그를 빌드해보세요.






나의 말:
일단 jekyll-sitemap는 설치하고 _config.yml 파일을 수정하여 jekyll-sitemap을 추가했어. 
로컬에서는 블로그를 어떻게 빌드를 하지?
ChatGPT의 말:
ChatGPT
로컬에서 Jekyll 블로그를 빌드하려면 다음 단계를 따라주세요.

로컬 서버 실행: 프로젝트 폴더(예: C:\Users\khju\Documents\Project_TBD\JavierJu.github.io)에서 터미널을 열고 아래 명령어를 실행합니다.

bash
코드 복사
jekyll serve
또는, bundle을 사용 중이라면 다음 명령어를 사용해도 됩니다.

bash
코드 복사
bundle exec jekyll serve
빌드 및 서버 확인: 명령어가 실행되면 Jekyll이 블로그를 빌드하고 로컬 서버를 시작합니다. 이 과정이 완료되면 터미널에 Server address: http://127.0.0.1:4000와 같은 메시지가 나타납니다.

웹 브라우저에서 확인: 브라우저에서 http://127.0.0.1:4000을 열면 로컬에서 빌드된 블로그를 확인할 수 있습니다.

사이트맵 파일 확인: http://127.0.0.1:4000/sitemap.xml에 접속하여 sitemap.xml 파일이 잘 생성되었는지 확인하세요.

Jekyll 빌드 후 문제가 없다면, 변경 사항을 커밋하고 GitHub 저장소에 푸시하면 됩니다.






나의 말:


PS C:\Users\khju\Documents\Project_TBD\JavierJu.github.io> jekyll serve
C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/resolver.rb:354:in raise_not_found!': Could not find gem 'jekyll-admin' in locally installed gems. (Bundler::GemNotFound)
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/resolver.rb:445:in block in prepare_dependencies'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/resolver.rb:420:in each'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/resolver.rb:420:in map'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/resolver.rb:420:in prepare_dependencies'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/resolver.rb:63:in setup_solver'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/resolver.rb:28:in start'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/definition.rb:633:in start_resolution'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/definition.rb:306:in resolve'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/definition.rb:582:in materialize'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/definition.rb:193:in specs'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/definition.rb:260:in specs_for'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler/runtime.rb:18:in setup'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/bundler-2.5.22/lib/bundler.rb:164:in setup'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/jekyll-4.3.4/lib/jekyll/plugin_manager.rb:52:in require_from_bundler'
        from C:/Ruby33-x64/lib/ruby/gems/3.3.0/gems/jekyll-4.3.4/exe/jekyll:11:in <top (required)>'
        from C:/Ruby33-x64/bin/jekyll:32:in load'
        from C:/Ruby33-x64/bin/jekyll:32:in <main>'
PS C:\Users\khju\Documents\Project_TBD\JavierJu.github.io> bundle exec jekyll serve
Could not find gem 'jekyll-admin' in locally installed gems.
Run bundle install to install missing gems.
ChatGPT의 말:
ChatGPT
이 오류는 jekyll-admin 플러그인이 로컬에 설치되지 않아 발생합니다. jekyll-admin 플러그인을 포함한 모든 필요 Gem을 설치하려면, 다음 단계를 따르세요:

bundle install 실행: 프로젝트 디렉토리(C:\Users\khju\Documents\Project_TBD\JavierJu.github.io)에서 아래 명령어를 실행하여 필요한 모든 Gem을 설치하세요.

bash
코드 복사
bundle install
이 명령은 Gemfile에 정의된 모든 Gem을 설치합니다.

서버 실행: 설치가 완료되면, 다시 아래 명령어를 실행하여 Jekyll 서버를 시작합니다.

bash
코드 복사
bundle exec jekyll serve
이제 로컬 서버가 잘 작동하여 http://127.0.0.1:4000에서 블로그를 확인할 수 있을 것입니다.






나의 말:
jekyll-sitemap 설치는 해당 블로그 루트폴더에 이동해서 설치해야하는건가?
ChatGPT의 말:
ChatGPT
네, jekyll-sitemap 설치는 블로그의 루트 폴더에서 진행하는 것이 좋습니다. 이렇게 하면 설치된 플러그인이 해당 프로젝트의 Gemfile 및 _config.yml 파일과 연동되어 빌드 과정에서 자동으로 적용됩니다.

루트 폴더에서 다음 명령어를 실행하여 필요한 플러그인들을 설치할 수 있습니다:

bash
코드 복사
bundle install
bundle install 명령어는 Gemfile에 정의된 모든 플러그인을 현재 프로젝트에 맞게 설치합니다. 이후 jekyll serve로 로컬 서버를 시작할 수 있습니다.






나의 말:
 sitemap.xml 파일은 루트 디렉토리에 자동으로 생성되는건가? jekyll-sitemap을 설치해도 파일이 생성 안된거 같은데?
ChatGPT의 말:
ChatGPT
네, jekyll-sitemap 플러그인을 설치하고 설정했다면 Jekyll이 빌드 시에 자동으로 sitemap.xml 파일을 생성합니다. 하지만 이 파일은 프로젝트 루트가 아닌 _site 폴더에 위치합니다. _site는 Jekyll이 빌드된 정적 파일들을 저장하는 디렉토리로, 실제로 GitHub Pages에 배포될 파일들이 저장되는 곳입니다.

sitemap.xml 생성 확인 방법
Jekyll 빌드 또는 서버 실행:

bash
코드 복사
bundle exec jekyll serve
또는

bash
코드 복사
jekyll build
_site 폴더 확인: 빌드 후 프로젝트 폴더 내의 _site/sitemap.xml 파일이 생성되었는지 확인하세요.

로컬 서버에서 확인: 로컬 서버를 실행 중이라면 웹 브라우저에서 http://127.0.0.1:4000/sitemap.xml에 접속해 사이트맵 파일을 확인할 수 있습니다.

_site 폴더에 sitemap.xml이 생성되지 않았다면, _config.yml 파일에 설정이 올바른지 다시 한 번 확인해 보세요.






나의 말:
성공적으로 검색엔진 결과에 노출되고 있는지 어떻게 확인할 수 있을까?
ChatGPT의 말:
ChatGPT
검색엔진에 성공적으로 노출되었는지 확인하는 방법은 다음과 같습니다.

1. Google에서 사이트 검색
Google 검색창에 다음과 같이 입력하여 사이트가 인덱싱되었는지 확인할 수 있습니다:

plaintext
코드 복사
site:your-github-username.github.io
예: site:javierju.github.io

이 검색 결과에 페이지들이 나타난다면 사이트가 Google에 인덱싱된 것입니다.

2. Google Search Console에서 확인
Google Search Console로 이동하여 프로젝트를 추가한 후 색인 현황 탭을 통해 인덱싱 상태를 모니터링할 수 있습니다.
색인된 페이지 수를 확인하고, 문제가 발생한 URL이나 색인되지 않은 페이지가 있다면 Search Console이 오류에 대한 세부 정보를 제공합니다.
3. URL 검사 도구 사용
Search Console 내 URL 검사 도구를 사용하여 특정 페이지 URL을 입력하면, 해당 URL이 인덱싱되었는지 확인할 수 있습니다. 인덱싱이 되어 있지 않다면, 여기에서 직접 색인 요청도 가능합니다.

4. 구글 애널리틱스 설정 (선택 사항)
사이트에 Google Analytics를 추가해 방문자 데이터를 모니터링하면, 실제 트래픽을 통한 검색엔진 노출 상태를 파악할 수 있습니다.

이러한 방법들을 통해 사이트의 노출 상태를 꾸준히 확인하면서, 필요에 따라 Search Console을 통해 색인 요청을 추가할 수 있습니다.



홈페이지 하단에 FEED 라는 것이 있는데 feed.xml 는 어떤 역할을 하는거지?
ChatGPT의 말:
ChatGPT
feed.xml은 웹사이트의 최신 콘텐츠(주로 블로그 게시글이나 뉴스)를 구독할 수 있도록 하는 RSS 피드 파일입니다. 웹사이트에 새로운 콘텐츠가 추가될 때, feed.xml 파일이 자동으로 업데이트되어 구독자가 변경된 내용을 쉽게 확인할 수 있습니다.

RSS 리더 앱이나 이메일 구독 서비스는 feed.xml을 통해 콘텐츠를 가져오고, 구독자는 앱 내에서 해당 콘텐츠를 볼 수 있게 됩니다. 이를 통해 사용자들은 웹사이트를 직접 방문하지 않고도 최신 업데이트를 받아볼 수 있습니다.

feed.xml 파일은 일반적으로 다음과 같은 정보를 포함합니다:

웹사이트 기본 정보: 사이트 제목, 설명, 링크 등
게시글 목록: 각 게시글의 제목, 요약, 게시일, 링크 등
이 XML 형식은 표준화되어 있어 다양한 RSS 리더와 호환됩니다. Jekyll 같은 정적 사이트 생성기에서는 자동으로 feed.xml 파일을 생성해주는 플러그인을 지원하기도 합니다.






나의 말:
jekyll-feed를 설치하는게 좋을까?
ChatGPT의 말:
ChatGPT
메모리 업데이트됨
네, jekyll-feed 플러그인을 설치하는 것은 좋은 선택입니다. 이 플러그인은 Jekyll 사이트에 feed.xml을 자동으로 생성해 주기 때문에 수동으로 작성할 필요 없이 간편하게 RSS 피드를 제공할 수 있습니다. GitHub Pages에서도 기본적으로 지원되어 별도의 설정 없이 사용할 수 있고, 사이트가 업데이트될 때마다 자동으로 피드도 갱신됩니다.

설치하려면 Gemfile에 jekyll-feed를 추가하고, _config.yml 파일에 아래와 같이 플러그인을 설정해 주세요.

Gemfile 수정:

ruby
코드 복사
gem "jekyll-feed"
_config.yml 수정:

yaml
코드 복사
plugins:
  - jekyll-feed
설치 후에는 자동으로 feed.xml 파일이 생성되며, 방문자들이 RSS 리더로 쉽게 구독할 수 있게 됩니다.






나의 말:
터미널 해당 폴더에서 gem "jekyll-feed" 를 실행하면 되나?
ChatGPT의 말:
ChatGPT
gem "jekyll-feed"를 터미널에 그대로 입력하면 실행되지 않습니다. jekyll-feed를 설치하려면 Gemfile 파일을 직접 수정하고, 그다음에 bundle install 명령어를 실행해야 합니다.

아래 단계에 따라 설치해 보세요.

Gemfile 수정
프로젝트 폴더 내의 Gemfile 파일을 열어 다음 줄을 추가합니다.

ruby
코드 복사
gem "jekyll-feed"
의존성 설치
터미널에서 해당 폴더로 이동한 후, 아래 명령어를 실행하여 jekyll-feed를 설치합니다.

bash
코드 복사
bundle install
이 명령어는 Gemfile에 정의된 모든 gem을 설치합니다.

_config.yml 파일 수정
_config.yml 파일에도 아래와 같이 플러그인 목록에 추가해 줍니다.

yaml
코드 복사
plugins:
  - jekyll-feed
이후 Jekyll 사이트를 다시 빌드하거나 배포하면 feed.xml 파일이 자동으로 생성됩니다.






나의 말:
RSS 피드를 제공한다는게 어떤 의미야?
ChatGPT의 말:
ChatGPT
RSS 피드를 제공한다는 것은 웹사이트의 콘텐츠를 구독할 수 있는 형식으로 제공한다는 의미입니다. RSS 피드는 주로 뉴스나 블로그에서 사용하는 XML 형식의 파일로, 웹사이트에서 새로운 콘텐츠(게시물, 기사 등)가 업데이트될 때 이를 구독자에게 자동으로 알려주고, 내용을 확인할 수 있게 해줍니다.

RSS 피드의 주요 특징과 의미
자동화된 업데이트 알림
웹사이트 방문자가 RSS 피드를 구독하면, 새로운 콘텐츠가 게시될 때마다 RSS 리더(또는 이메일, 알림 앱 등)를 통해 자동으로 알림을 받게 됩니다.

콘텐츠 배포 및 확장성
RSS 피드를 통해 블로그나 뉴스 기사를 여러 플랫폼에 쉽게 배포할 수 있어, 웹사이트에 직접 방문하지 않아도 새로운 콘텐츠를 확인할 수 있습니다. 구독자는 RSS 리더, 이메일 구독, 모바일 앱 등 다양한 매체를 통해 내용을 볼 수 있습니다.

피드의 데이터 구조
일반적으로 RSS 피드는 각 게시물의 제목, 요약, 링크, 게시일 등의 정보를 포함하여 간결하게 콘텐츠를 제공하며, XML로 구조화되어 다양한 프로그램이나 앱에서 쉽게 읽을 수 있습니다.

Jekyll-feed 플러그인을 통해 얻는 이점
jekyll-feed 플러그인을 설치하면, Jekyll 사이트에서 자동으로 feed.xml 파일이 생성됩니다. 이 파일을 통해 방문자들은 웹사이트의 최신 콘텐츠를 구독할 수 있고, 웹사이트는 별도의 설정이나 수동 관리 없이도 최신 피드를 자동으로 제공하게 됩니다.

feed.xml 파일이 생성되었는데 블로그에서 클릭하면(https://javierju.github.io/feed.xml) 아래와 같이 페이지에 표시되는데, 맞는거야? 구독자가 어떻게 구독을 하는거지?

This XML file does not appear to have any style information associated with it. The document tree is shown below.
<feed xmlns="http://www.w3.org/2005/Atom">
<script/>
<script/>
<generator uri="https://jekyllrb.com/" version="3.10.0">Jekyll</generator>
<link href="https://javierju.github.io/feed.xml" rel="self" type="application/atom+xml"/>
<link href="https://javierju.github.io/" rel="alternate" type="text/html"/>
<updated>2024-10-26T13:08:33+00:00</updated>
<id>https://javierju.github.io/feed.xml</id>
<title type="html">JavierJu Devlog</title>
<subtitle>40대 아재의 프로그래밍 독학 블로그!</subtitle>
<author>
<name>Javier Ju</name>
ChatGPT의 말:
ChatGPT
네, feed.xml 파일이 표시되는 형태는 정상입니다. RSS 피드는 웹 브라우저에서 스타일 없이 XML로 보이며, 사람보다는 RSS 리더나 구독 프로그램이 읽을 수 있도록 만들어진 데이터 형식입니다. 그래서 XML 형식 그대로 노출되는 게 맞습니다.

구독하는 방법
일반 방문자나 구독자가 RSS 피드를 구독하려면 다음과 같은 방법을 사용할 수 있습니다.

RSS 리더 앱 사용
사용자가 RSS 리더(예: Feedly, Inoreader 같은 앱 또는 웹사이트)에 가서 피드 URL을 추가하면, 해당 앱이 feed.xml 파일을 주기적으로 확인하여 새로운 게시물이 있을 때 구독자에게 알림을 보냅니다.

예: https://javierju.github.io/feed.xml URL을 복사하여 RSS 리더에서 추가
브라우저 확장 프로그램 사용
일부 웹 브라우저는 RSS 피드를 지원하는 확장 프로그램이 있어, 사용자가 피드 주소를 입력하여 구독하거나 피드를 자동으로 감지하여 구독을 쉽게 할 수 있게 합니다.

이메일 구독 서비스
RSS 피드를 이메일로 구독할 수 있는 서비스(예: Blogtrottr)를 통해 사용자는 새로운 게시글이 등록될 때 이메일로 알림을 받을 수 있습니다.

참고
feed.xml이 웹 브라우저에서 스타일 없이 표시되는 것이 어색해 보일 수 있지만, RSS 구독 프로그램은 이 XML 파일을 구조적으로 해석하여 깔끔하게 최신 게시물을 나열하여 보여줍니다.




