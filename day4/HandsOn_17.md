# Hands-On 17

> **Attention!** At this point we've skipped a couple of topics in the original tutorial, because they aren't that relevant for us. If you notice discrepancies with what you have and what the Hands-Ons from now on assume you have, it would be a good idea to use [this solution project](../sample-files/chapter9solution) as your new basis.

In this Hands-On example, you are going to make a call to Twitter to get a list of your latest tweets and output them to the contact page.

> **Note:** This Hands-On originally used the Twitter api v1, which is no longer supported. The code sample now uses a static XML file hosted on our server.

**Tags Used**: [\<cfhttp>](https://helpx.adobe.com/coldfusion/cfml-reference/coldfusion-tags/tags-g-h/cfhttp.html), [\<cfloop>](https://helpx.adobe.com/coldfusion/cfml-reference/coldfusion-tags/tags-j-l/cfloop.html), [\<cfoutput>](https://helpx.adobe.com/coldfusion/cfml-reference/coldfusion-tags/tags-m-o/cfoutput.html)

**Functions Used**: [dateFormat](https://helpx.adobe.com/coldfusion/cfml-reference/coldfusion-functions/functions-c-d/DateFormat.html)

1. Open up the `/www/contact.cfm` file in your code editor.
1. First, make a `<cfhttp>` call to get the data from the sample Twitter XML. Create a `<cfhttp>` tag directly after the `<cfset>` block. Your code should be on or around line 7. The `<cfhttp>` tag should have the following attributes:

    * **url**: http://download.bokowsky.net/learncfinaweek/twitter.xml
    * **method**: get
    * **result**: twitterFeed

1. The code should look similar to this:

    ```cfml
    <cfhttp url="http://download.bokowsky.net/learncfinaweek/twitter.xml" method="get" result="twitterFeed">
    ```

1. Now that you have the feed data in the `twitterFeed` variable, you need to output it on the page. To do this, loop over the XML data that was received and create a `<cfloop>` tag. Locate the comment that reads `Twitter Output` and create the `<cfloop>` tag before the first `<li>` tag. The `<cfloop>` tag should have the following properties:
    * **array**: #xmlParse(twitterFeed.fileContent).statuses.status#
    * **index**: feedItem
1. Place the closing `</cfloop>` tag after the closing `</li>` tag.
1. Inside the `<li>` tags, place the following line of code:

    ```cfml
    #dateformat(feedItem.created_at.xmlText, 'mm/dd/yyyy')# - #feedItem.text.xmlText#
    ```

1. Now wrap the entire `<cfloop>` block in a `<cfoutput>` tag so that the variables will be displayed to the user. Your code should look similar to this:

    ```cfml
    <ul>
        <cfoutput>
            <cfloop array="#xmlParse(twitterFeed.fileContent).statuses.status#" index="feedItem" >
                <li>
                    #dateformat(feedItem.created_at.xmlText, 'mm/dd/yyyy')# - #feedItem.text.xmlText#
                </li>
            </cfloop>
        </cfoutput>
    </ul>
    ```

1. Open up the `/www/contact.cfm` page in your browser and notice that a couple of tweets are now displaying under the 'Latest Tweet' heading.
