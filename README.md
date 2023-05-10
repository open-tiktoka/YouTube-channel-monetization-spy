# is-youtube-channel-monetized-extension



adapt from Source code for "Is YouTube Channel Monetized?" extension.


*  add  more browser support
* skip youtube preminuim plan user
* support in youtube video page

1. detect is_monetization_enabled 
2. detect adsense

https://stackoverflow.com/questions/57792612/is-it-possible-to-extract-the-adsense-publisher-id-of-a-youtube-channel-video

you want to know the Adsense ID of user-owned current YouTube Channel. All you need is view source the video page and find ca-yt-host-pub if current youtube video has permission to run google ad.

I will come up with ca-yt-host-pub-[adsenseid].

If video or youtube channel is not eligible for google ads you will not find any entry for same.

```
var adUnit = document.getElementsByClassName("section-below-title")[0];
var parentElement = adUnit.childNodes[0];

// Create script element
var script = document.createElement('script');
script.async = true;
script.src = "https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js";
parentElement.appendChild(script);

// Create INS element
var ins = document.createElement('ins');
ins.className = "adsbygoogle";
ins.style = "display:inline-block;width:300px;height:250px;";
ins.setAttribute('data-ad-client', "ca-pub-XXXXXXXXXXXXXXXX");
ins.setAttribute('data-ad-slot', "XXXXXXXXXX");

parentElement.appendChild(ins);
(adsbygoogle = window.adsbygoogle || []).push({});


```
4. detect ads

```
    if(document.getElementsByClassName("video-stream html5-main-video")[0]!==undefined){
        let ad = document.getElementsByClassName("video-ads ytp-ad-module")[0];
        let vid = document.getElementsByClassName("video-stream html5-main-video")[0];
        if(ad==undefined){
            pbRate = vid.playbackRate;
        }
```

	'.googleadservices.com': ['googleAds'],
	'pagead2.googlesyndication.com': ['googleAds'],
	'.doubleclick.net': ['googleAds'],
  
  function check_adsense() {
    const ad_string = 'adsense'
    const ad_string_2 = 'pagead2'
    var body_HTML = document.body.innerHTML
    var searchPattern = new RegExp('(\\w*' + ad_string + '\\w*)|(\\w*' + ad_string_2 + '\\w*)', 'gi')
    var matches = body_HTML.match(searchPattern)

    if (matches === null ) {
        add_symbol('demonetized')
    } else {
        add_symbol('monetized')
    }
}

function add_symbol(image) {
    var symbol = document.createElement("img")
    symbol.id = 'monetization_symbol'
    var eow_title = document.getElementById('eow-title')
    var watch_title_container = document.querySelector('#watch-headline-title > h1')
    var old_title = eow_title.textContent

    if (image == 'monetized') {
        symbol.src = chrome.extension.getURL('icons/monetized.png')
        symbol.title = 'This video is monetized'
    } else {
        symbol.src = chrome.extension.getURL('icons/demonetized.png')
        symbol.title = 'This video is not monitized or demonetized'
    }

    watch_title_container.insertAdjacentElement('afterbegin', symbol)
}
https://github.com/Tominous/yt-monetization-extension

const YOUTUBE_AD_REGEX = /(doubleclick\.net)|(adservice\.google\.)|(youtube\.com\/api\/stats\/ads)|(&ad_type=)|(&adurl=)|(-pagead-id.)|(doubleclick\.com)|(\/ad_status.)|(\/api\/ads\/)|(\/googleads)|(\/pagead\/gen_)|(\/pagead\/lvz?)|(\/pubads.)|(\/pubads_)|(\/securepubads)|(=adunit&)|(googlesyndication\.com)|(innovid\.com)|(youtube\.com\/pagead\/)|(google\.com\/pagead\/)|(flashtalking\.com)|(googleadservices\.com)|(s0\.2mdn\.net\/ads)|(www\.youtube\.com\/ptracking)|(www\.youtube\.com\/pagead)|(www\.youtube\.com\/get_midroll_)/;



5. 


Available on:
- [Chrome Web Store](https://chrome.google.com/webstore/detail/is-youtube-channel-moneti/ijonaoomgjhjacfmipjlfdobddhfnkjn)
- [Firefox Add-ons](https://addons.mozilla.org/en-US/firefox/addon/is-youtube-channel-monetized/)
