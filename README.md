# Chat Tags
Chat Tags is a mark-up parser for _Special:Chat_.

## Description
Chat Tags allows users to add formatting to their in-chat messages. Such formatting includes the ability to add colors to their messages, change the font of their messages, alter the font size, alter the font weight, and the background of their messages.

This gives users the creative freedom to better express themselves while in chat.

Users are now able to bold important sections of their messages or even highlight them using colors and backgrounds instead of using all caps. They are also able to italicize quotes and what ever else they deem necessary.

Put together this script enhances chats of all purposes.

# Installation
Chat Tags can easily be installed by editing the following line into your wiki's __MediaWiki:Chat.js__ page.

`importScriptPage('MediaWiki:ChatTags/code.js', 'shining-armor');`

Or if your wiki is already using an `importArticles` statement you can add the following line to your `scripts` section.

`'u:shining-armor:MediaWiki:ChatTags/code.js',`

Additionally, if you would like to enable the image or youtube tag you will need to add the following line __ABOVE__ the script import.

`var chatags = { images: true, videos: true };`

# Usage
To use Chat Tags on your wiki after installation one must only wrap their text in the designated tags that you will find below. Chat Tags functions much like HTML in that it requires a beginning tag that shows where you wish to start formatting and and ending tag to show when to stop formatting. If you do not include both of these tags then your text will be ignored by the script.

__Tags must end in the order that they start__

## Tags


* Text color
    * `[c <color code>]<message>[/c]`
* Background color
    * `[bg <color code>]<message>[/bg]`
* Font
    * `[font &lt;font name>]<message>[/font]`
* Code tag
    * `[code]<message>[/code]`
* Bold text
    * `[b]<message>[/b]`
* Italicize text
    * `[i]<message>[/i]`
* Big text
    * `[big]<message>[/big]`
* Small text
    * `[small]<message>[/small]`
* Subscript
    * `[sub]<message>[/sub]`
* Superscript
    * `[sup]<message>[/sup]`
* Strike through
    * `[s]<message>[/s]`
* Underline
    * `[u]<message>[/u]`
* Image
    *__Note: Leave the http:// off of the link__
    * `[img <image>]`
* Youtube
    * __Note__: `www.youtube.com/watch?v=`**uQzGxQxn84Y**
    * `[yt <video ID>]`

# Developers
ChatTags is very much extensible and allows developers to easily add and remove tags as they see fit.

In order to add a tag you must first import ChatTags using the following line.

`importScriptPage('MediaWiki:ChatTags/code.js', 'shining-armor');`

After doing this you must add an entry to the tags object in order to do this you must pick a name that has not been previously chosen. All default tags are listed above. After selecting the name for your tag you must then decide whether it is stand-alone or a pair.

A stand alone tag only require and opening tag (see: img, yt).

A pair tag  requires both an opening and closing tag (see: c, u, bg)

After deciding the structure of your tag you must decide if you want it to accept a parameter or not. After deciding this you want to create your object entry. For example purposes we will be creating a user tag that turns their text color purple.

To add an entry we must do the following __*below*__ our import.

    //... import ...
    chatags.tags['user'] = function(s,t) {        // S is the user string, T is the tag
        if (t.charAt(0) === '/') {                // We have been given the ending tag
            s = s.replace('[/user]', '</span>');
        } else {                                  // We have been given the opening tag
            s = s.replace('[user]', '<span style="color:purple">');
        }
        return s;                                 // Return the user string for further parsing
    };


Now, let's say that we want to let the user enter their own color, we'd need to do something like the following.

    //... import ...
    chatags.tags['user'] = function(s,t) {
        if (t.charAt(0) === '/') {
            s = s.replace('[/user]', '</span>');
        } else {
            try {
                t = t.split(' ');
                s = s.replace('[user' + t[1] + ']', '<span style="color:' + mw.html.escape(t[1]) + ';">');
            } catch(e) { console.log(e) }
        }
        return s;
    };

When dealing with user input you must _always__ use `mw.html.escape()` to escape the input to prevent script injection.

