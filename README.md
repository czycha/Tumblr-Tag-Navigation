Tumblr-Tag-Navigation
=====================

How to add user-defined tags to the navigation of a Tumblr theme.

## Step 1 - Create a Tumblr text variable

In your head, insert `<meta name="text:Tags separated by commas" content=""/>`. This is a Tumblr function to state that there is a text variable that the user can define that is named "Tags separated by commas".

## Step 2 - Create a secret element

Most Tumblr variables can be used in Javascript simply by prefixing them with `JS`. This does not work with custom variables (or I haven't found a way to make them work, at least). So here's my way around that. Insert something like this somewhere on the page: `<span id="tags" style="display:none">{text:Tags separated by commas}</span>`. This creates a span element with the id of "tags" that contains the user-defined tags within its inner HTML. This will not display because of the `style="display:none"`.

## Step 3 - Parse the tags with Javascript

Now we have to find the individual tags that your theme owner wants to have as navigation. This can take different routes depending on if you use [jQuery](http://www.jquery.com) or not. I highly suggest that you use jQuery for making themes but if you choose not to, that's fine.

### Without jQuery

    var tags=document.getElementById('tags').innerHTML.replace(/,(\s*,)*/g,',').replace(/\s*,\s*/g,',').replace(/,$/,'').replace(/^\s*/,'').replace(/\s*$/,'').split(',');
    
### With jQuery

    var tags=jQuery('#tags').html().replace(/,(\s*,)*/g,',').replace(/\s*,\s*/g,',').replace(/,$/,'').replace(/^\s*/,'').replace(/\s*$/,'').split(',');
    
The only difference here is how to receive the tags (through the `innerHTML` of straight Javascript or through the `.html()` method of jQuery). The rest is the same.

All of the `.replace()` functions make a standard comma separation system (something like `tag 1,tag 2,tag3`. The reason for this is because with different users, there are different mistakes that can be made (`tag 1  , tag 2,   tag 3,` for example). It then splits the string based on the commas, creating an array with each element corresponding to a tag (`tags=["tag 1", "tag 2", "tag 3"]`).

## Step 4 - Add your tags to the navigation.

This is where using jQuery gives you an advantage but if you choose not to use it, *c'est la vie*. This part is where you can get creative (I'm making it bare bones for the sake of the example). For this example, there exists an element with an id of "navi" where we want to display our tag navigators.

### Without jQuery

    for(var x in tags) {
     var a=document.createElement('a');
     a.href="/tagged/"+escape(tags[x].replace(/ /g, '-'));
     a.innerHTML=tags[x];
     document.getElementById('navi').appendChild(a);
    }

This goes through each element of tags [line 1], creates an `a` element [line 2], sets the element's `href` attribute to the tag's page (note that tag pages replace spaces with dashes) [line 3], set the `innerHTML` of the element to the tag [line 4], and then appends the element to the element with the id of `navi` [line 5].

### With jQuery

    jQuery.each(tags, function() {
     jQuery('#navi').append('<a href="/tagged/'+escape(this.replace(/ /g, '-'))+'">'+this+'</a>');
    });

Iterates through the tags and performs a function for each element [line 1]. The function appends to the element with the id of `navi` a hyperlink with its `href` set to the tag's page and with the `innerHTML` set to the name of tag [line 2].

## Step 5 - Compile the Javascript into a function
## Without jQuery

    function addTags() {
     var tags=document.getElementById('tags').innerHTML.replace(/,(\s*,)*/g,',').replace(/\s*,\s*/g,',').replace(/,$/,'').replace(/^\s*/,'').replace(/\s*$/,'').split(',');
     for(var x in tags) {
      var a=document.createElement('a');
      a.href="/tagged/"+escape(tags[x].replace(/ /g, '-'));
      a.innerHTML=tags[x];
      document.getElementById('navi').appendChild(a);
     }
    }

## With jQuery

    function addTags() {
     var tags=jQuery('#tags').html().replace(/,(\s*,)*/g,',').replace(/\s*,\s*/g,',').replace(/,$/,'').replace(/^\s*/,'').replace(/\s*$/,'').split(',');
     jQuery.each(tags, function() {
      jQuery('#navi').append('<a href="/tagged/'+escape(this.replace(/ /g, '-'))+'">'+this+'</a>');
     });
    }

## Step 6 - Set the function to go off when the document is ready
### Without jQuery

    window.onLoad=addTags;

### With jQuery

    jQuery(document).ready(addTags);

