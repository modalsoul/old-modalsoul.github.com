---
layout: page
title: modal_soul debriefing
tagline: Tech Blog (BETA)
---
{% include JB/setup %}
<div class="row-fluid">
  <div class="span9">
    <ul class="posts">
      {% for post in site.posts limit:7 %} 
        <h6> <span>{{ post.date | date_to_string }}</span>&nbsp;&raquo;&nbsp;</h6>
        <h4><a href="{{ post.url }}">{{ post.title}}</a></h4>
        <p>{{ post.comment }}</p>
        <a href="{{ post.url}}">
    	  <img src='http://capture.heartrails.com/320x180/cool/shorten?{{BASE_PATH}}{{ post.url }}'>
        </a>
        <div id="right">
          <a href="https://twitter.com/share" class="twitter-share-button" data-url="http://modalsoul.github.io{{ post.url }}/" data-via="modal_soul" data-lang="ja">ツイート</a>
          <script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0];if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src="//platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");
          </script>
			
          <a href="http://b.hatena.ne.jp/entry/http://modalsoul.github.io{{ page.url }}/" class="hatena-bookmark-button" data-hatena-bookmark-title="{{ post.title }}" data-hatena-bookmark-layout="standard" title="このエントリーをはてなブックマークに追加">
            <img src="http://b.st-hatena.com/images/entry-button/button-only.gif" alt="このエントリーをはてなブックマークに追加" width="20" height="20" style="border: none;" />
          </a>
          <script type="text/javascript" src="http://b.st-hatena.com/js/bookmark_button.js" charset="utf-8" async="async">
          </script>
          
		  <div class="fb-like" data-href="http://modalsoul.github.io{{ post.url }}/" data-send="false" data-layout="button_count" data-width="450" data-show-faces="true">
          </div>
        </div>
      {% endfor %}
    </ul>
  </div>
  <div class="span3">
    <h4>Author</h4>
    <h5>modal_soul</h5>
    <p>I'm a software engineer, Scala programer, Android app developer.</p> 
    <a href="https://twitter.com/modal_soul">
      <img alt="Follow me on Twitter" src="/img/Twitter_lg.png" style="width: 130px;">
    </a>
    <a href="https://play.google.com/store/search?q=pub:modal_soul">
      <img alt="Get it on Google Play"
       src="https://developer.android.com/images/brand/en_generic_rgb_wo_45.png" />
    </a>
    <p>Certified Scrum Master  Certified Scrum Product Owner</p>
    <div style="float:left;">
      <img src="/assets/ScrumMaster_Logo_Seal.png" style="width: 65px; max-width: 100%; height: auto; float: left;" >
      <img src="/assets/Scrum_Product_Owner_Seal.png" style="width: 65px; max-width: 100%; height: auto; float: left;" >
    </div>
    <div>
      <!-- You also need to place a container where you'd like the Coderwall badges to render. -->
      <section class="coderwall" data-coderwall-username="modalsoul" data-coderwall-orientation="vertical"></section>
    </div>
    <a class="twitter-timeline" href="https://twitter.com/modal_soul" data-widget-id="364724609657999360">@modal_soul からのツイート</a>
    <script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");
    </script>

	<div>
    <script src="http://tool2.fxwill.com/amazon/set_amazon.php?ListId=JD4OW220A56Y&AssociatesId=modalsoul-22&txt=3&color=1">
    </script>
    <a href="http://aws.fxwill.com/" id="fxwillamazon">fxwill.com</a>
    <script charset="utf-8" src="http://widgets.twimg.com/j/2/widget.js"></script>
    <script>
		new TWTR.Widget({
		  version: 2,
		  type: 'profile',
		  rpp: 4,
		  interval: 30000,
		  width: 'auto',
		  height: 300,
		  theme: {
		    shell: {
	      background: '#333333',
	      color: '#ffffff'
	    },
	    tweets: {
	      background: '#000000',
	      color: '#ffffff',
	      links: '#0781eb'
	    }
	  	},
	  	features: {
	    	scrollbar: false,
	    	loop: false,
	    	live: false,
	    	behavior: 'all'
	  	}
		}).render().setUser('modal_soul').start();
  </script>
  </div>
  <script type="text/javascript"><!--
		google_ad_client = "ca-pub-8642536258449297";
		/* modalsoul1 */
		google_ad_slot = "8173659218";
		google_ad_width = 160;
		google_ad_height = 600;
		//-->
		</script>
		<script type="text/javascript"
		src="http://pagead2.googlesyndication.com/pagead/show_ads.js">
  </script>
  </div>
</div>
