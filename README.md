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


https://googleads.g.doubleclick.net/pagead/ads?cust_gender=1&cust_age=1001&lact=-1&ytdevicever=ytdevicever_2.20190830.05.01&client=ca-pub-6219811747049371&correlator=7680223626982580911&ad_block=3&host=ca-yt-host-pub-0161219093041726&loeid=23757412,23801214,23802811,23825187,23840454&num_ads=1&video_doc_id=yt_KBXTnrD_Zs4&ad_type=text&channel=PyvWatchInRelated%2BPyvWatchNoAdX%2BPyvYTWatch%2Bafv_user_id_XuqSBlHAE6Xw-yeJA0Tunw%2Bafv_user_linustechtips%2Bnon_lpw%2Bpw%2Byt_bs3%2Byt_cid_563326%2Byt_mpvid_BQaqvwtEkr3N-Vog%2Byt_no_ap%2Bytdevice_1%2Bytdevicever_2.20190830.05.01&hl=en&url=http%3A%2F%2Fwww.youtube.com%2Fvideo%2FKBXTnrD_Zs4&output=js&ea=0&eae=2&adk=511001906&pyv=1&top=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DKBXTnrD_Zs4%26app%3Ddesktop&loc=EMPTY&dbp=ChZmMk1sUGFvOWhNXzQxODVubWxqWGJnEAEgASgAOAI&isw=0&ish=0&yt_pt=APb3F29OhgLzyszqYbU31fmkEgs0AccKCG0aOJXYOcyJLapYBsdDy_HexgVuEvTteHE1kKgddrYC3i1Qgf4yNQcTMPDGVC1b9xGD7U4d4YWUjxn7ES4XVIPIie-x3ppb1TbzKQF5HnknccafebzS8qTeIvotbAbqyOoP9_pYI-KSq8e2R9QvqmL6V4kgkVYfVOy6PL7EL2xznIBaD5bWf9nypJFUAZaokgQFMgLktQ&pucrd=APb3F28coEKL-ngqMchgLsw9cO1G46pzUkBYVSf4yZbajqJAeLNYkCYJ3AntOsWoZcUpLH0Nq6pV0EBaKsg82QrDAXodYWwgyrm9a3nIzw&dt=1567306839089&flash=0&frm=0&u_tz=-420&u_his=4&u_java=false&u_h=1080&u_w=1920&u_ah=1040&u_aw=1920&u_cd=24&u_nplug=2&u_nmime=2&bc=31&bih=973&biw=1920&brdim=1920%2C0%2C1920%2C0%2C1920%2C0%2C1920%2C1040%2C1920%2C973&vis=1&wgl=true&dff=times%20new%20roman&dfs=16&ppjl=u&rsz=%7C%7Cn%7C&dssz=40&icsg=68669144847&mdo=0&mso=0&iag=0&lact=1070



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
