Manual advanced configuration (about:config)
--------------------------------------------

    user_pref("general.useragent.override", "");
    user_pref("mailnews.headers.showUserAgent", true);
    user_pref("mailnews.wraplength", 0);

If support for the Markdown Here extension is needed, the first preference
needs to be like this:

    user_pref("general.useragent.override", "Thunderbird");
