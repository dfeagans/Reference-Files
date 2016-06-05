# General Html/CSS Notes #
Rule 1:HTML for structure, CSS appearance, and JS for behavior

## [Center/Scaling/Full Width Background](http://css-tricks.com/perfect-full-page-background-image/) ##
Used in [Creo Automation Page](https://github.com/dfeagans/Creo_v1)
```css
html { 
  background: url(images/bg.jpg) no-repeat center center fixed; 
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}
```

## [Transparent/Opaque Divs and Text](http://24ways.org/2009/working-with-rgba-colour/) ##

Basically the `div{opacity=0.5};` css line makes everything in the div, including text transparent. 

`background-color:rgba(0,0,0,0.5);` on the other hand lets you specificy the same opacity (0.5) but only on the div, not the text. It needs to be preceeded with a standard `background-color=#000000;` or something to set the fall-back color in-case the browser can't transparency. CSS ignores anything it doesn't understand, so it sets the color with the fallback first and only makes it transparent if the browser supports it.
 
## [Footer that Stays on Bottom](http://fortysevenmedia.com/blog/archives/making_your_footer_stay_put_with_css/) ##
If you know the page will never be over one sheet long you can use a `width: 100%;` div that is spaced off the `bottom:10px;` or whatever. Then use `position:fixed;` to make it stay there no matter the zoom. 
 
## Use CDNs for JQuery and Other Useful Libraries ##
Google hosts a bunch of frameworks [here](https://developers.google.com/speed/libraries/devguide)

Sometimes people host them locally on their own web server, but if you use the Google one the library is probably already loaded in their cache and it will speed up the site. To use the CDN, the syntax is `<script src="//ajax.googleapis.com/ajax/libs/jquery/1.10.1/jquery.min.js"></script>` . Note that the link starts with "//" that allows the browser to determine which protocol it needs (HTTP/HTTPS). It presents a problem when you are opening the webpage from file because it things the // is specifying a directory on your local computer. You'll have to add the http or https if that's the case. It's possible to always load the latest jquery code too, see [link](http://stackoverflow.com/questions/441412/is-there-a-link-to-the-latest-jquery-library-on-google-apis).

There is one place where it's useful to use locally hosted libraries: If you're hosting the site only on your own intranet. There is no way google will beat your local intranet. I guess you have to weigh the likelihood of it already being cached still though? Google also hosts some themes for the JQuieryUI program.

For example the Creo_v1 project jQuery is used, it's easiest to just open that file locally within Creo when necessary, so I had to change the CDN line to <script src="((**http://**...".

## Fade Screen Upon Error and Display Centered Message ##
The following code is used to turn the screen 50% opaque with an error message centered, at the top of the screen:

```html
HTML:
  <div id="errorScreen">THIS SITE MUST BE OPENED IN THE CREO INTEGRATED BROWSER</div>

CSS:
  #errorScreen{
    background:#000;
    position:fixed;
    top:0; left:0;
    width:100%;
    height:100%;
    z-index:10;
    text-align: center;
  }

JS: 
  try{
    //Code that is likely to fail
  }
  catch(er){
    $('#errorScreen').fadeTo("slow", 0.5);
  }
```

Note: I know it sounds stupid, but fadein() has to operate on something invisible. That means to get an element to fade in on side load you'd have to hide it first i.e. `$('#menu').hide().fadeIn(1000);`.
