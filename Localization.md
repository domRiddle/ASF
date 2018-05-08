# Localization

ASF is powered by Crowdin service, which makes it possible for everybody to help translating ASF into all languages spoken worldwide. For more detailed explanation how Crowdin works, please check out **[Crowdin introduction](https://support.crowdin.com/crowdin-intro/)**.

If you're interested in what is currently going on, you can check **[ASF Crowdin activity](https://crowdin.com/project/archisteamfarm/activity_stream)**.

---

## Scope

Our platform supports localization of our main ASF program, as well as whole localizable content that we offer together with it. This includes especially our web config generator, our IPC GUI, as well as our wiki. All of that is possible to translate through convenient crowdin interface.

---

## Signing up

If you'd like to help with ASF, either by translating, reviewing or approving translations, please sign up on our **[Crowdin project page](https://crowdin.com/project/archisteamfarm)**. Registration is easy and absolutely free! After logging in you can pick languages that you'd like to get assigned to, then proceed to ASF strings and help the rest of the community with translating ASF into all most popular languages!

---

### Translating

If the language of your choice is still missing some strings, you can grab them and start working on the translation. We tried to do our best in terms of flexibility of the translations, therefore many strings include extra variables that ASF will provide during runtime - those are enclosed in brackets with a number, such as `{0}`. This allows you to alter default ASF format of the string, e.g. by moving ASF-provided variable in a place that satisfies your language and your translation, instead of being forced to strict context and format. This is especially important in RTL languages, such as Hebrew.

For example, you could have a string like:

> We have {0} games to idle.

But based on your language, following sentence could make more sense:

> The number of games to idle is equal to {0}.

The flexibility is provided specially for you, so you can slightly reword ASF sentence to fit your language better and move ASF-provided number or other information in a place that fits your translation (instead of translating each part independently). This improves overall translation quality.

---

### Reviewing

If your string was already translated by somebody else, you can vote for it. Voting makes it possible to choose the best variant of the translation, instead of sticking with initial suggestion - this enhances overall translation quality even further. You can vote on already available suggestions, or suggest your own translation, which will go through the same process. Eventually, final string will be chosen either based on most voted suggestion, or as a choice of proofreader selected for that language who personally approves given translation (based on your votes as well).

**You do not need approval to see your translated strings in ASF**. Approval simply means that somebody trusted reviewed the content, as in - picked the final version of the translation. It's totally fine to have not-approved community-driven translations, where you vote for the best one. As long as it's translated, everything is fine! And if you think that current translation is bad, you can always vote for the better one, or suggest one yourself! 👍 

---

### Proof-reading

It's a good idea to have a consistent translation, even if it could potentially take freedom from community review/voting process explained above. This is mainly because incorrect translations that are not necessarily bad might get so many upvotes that it's no longer possible to suggest any better translation, even if somebody has such.

If you have past history of contributions on Crowdin or any other localization platform/service that we can verify and assume trustworthy, we're happy to give you a proof-reader access to given language you're contributing to, so you'll be able to approve given translation and make it consistent. Proof-reading is not an easy task, especially because ASF can be very "technical" from time to time and really difficult to translate, but we understand that it's often needed for a perfect translation. Therefore if you can help by proof-reading given language, **[let us know](https://crowdin.com/messages/create/13177432)**, but keep in mind that you'll need to back up your request with past localization contributions that we can verify (e.g. working with ASF localization on Crowdin, or with any other project). We might also allow more advanced users to pick up initial proof-reading, if we know them personally and they're capable of cooperating with the rest of the community in order to localize ASF in that language best.

General rules apply for proof-reading - do not rush, listen to your users, work as a project manager, resolve issues, ensure that you're making things better and not worse.

---

### Issues

If you have a problem with particular translation, e.g. you do not know how to translate it, approved translation is incorrect, you need more specific context, or likewise, please post a comment under specific string, and mark it with [X] Issue.

**Please avoid using issue mark if you do not need technical/development explanation or admin action**. You're free to use comments for discussion related to translation of given string, but issue should be used only when you need further technical explanation or admin correction, and it will typically involve somebody who do not even speak the language you're translating, so please stick with English when writing issue comment (so we can understand what the issue is).

There are currently 4 supported type of issues:
- General question - for everything else that doesn't fit any issue below. In general this type **should be avoided**, as if your problem does not fit, then it's very likely **not** a translation issue. Still, this option is available here for all other cases.
- Current translation is wrong - this should be used **only** if translation was pre-approved by proof-reader already, and you believe that it's wrong, for example it has a typo or you have a valid suggestion how to improve it. This type should never be used in translations that are powered by the community (voting), as in this case you should contact with user of given translation and ask him for correction, or simply vote for better translation, as stated in reviewing section.
- Lack of contextual information  - this is what you should use if you're not sure what part of ASF you're translating, what is the context of given string, or its purpose. This type should be used for ASF development only, it means you need technical assistance as you're not sure how you should translate given string.
- Mistake in the source string - this should be used only if you believe that original (English) string is incorrect. Quite rare, but I'm not speaking English natively either, so feel free to use it if you have a general idea how it could be improved.

---

### Translation progress

Every language has two states of completion - translation, and proof-reading.

Language is considered **translated** when its translation progress reaches 100%. At this point every localizable string used by ASF has proper meaning, which is great. However, that doesn't mean that there is no room for improvement - community voting is enabled all the time and you can still suggest better translation for already-translated parts, as well as vote for existing ones. Please note that fully-translated languages can still drop below 100% when we change existing strings or add new ones during development. You can set up appropriate crowdin notifications if you'd like to receive e-mail when this happens.

Selected languages might have appropriate proof-readers that validate translations and approve final versions. This is final pass after translation takes place and allows to further improve localization.

ASF will include given language **as soon as possible**, which means that it doesn't need to be approved, or even 100% translated. The actual strings that will be used are always the most popular ones in terms of the votes, unless chosen proofreader decided otherwise (rarily). Therefore, you can see your efforts included in the very next ASF release, as soon as translation is pushed to Crowdin - we typically merge localization updates the moment we're about to release new ASF version.

---

## Missing languages

By default ASF project has open translation only for top 30 languages that are spoken worldwide. If you'd like to add another one (or a local dialect to already available one), please **[let us know](https://crowdin.com/messages/create/13177432)** and we'll add it ASAP. We don't want to open several hundred different languages if nobody is going to translate them, that's why we limited it to some fair number. Please don't hesitate to contact us if you'd like to translate some not-listed language, it's very easy for us to add another one.

For a complete list of all available languages that ASF can be translated to, **[click here](https://support.crowdin.com/api/language-codes)**.

---

## Wiki localization

Our crowdin also allows you to localize our wiki itself. This is a very powerful tool, since you can also adapt the original text in order to make it point to new (localized) locations.

As you should know already, everybody can change wiki language by adding `-locale` string to any visited page. For example, instead of visiting **[FAQ](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ)**, you can visit **[FAQ-ru-RU](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ-ru-RU)** that contains FAQ translated into Russian (if available).

Now, ASF includes links on every page for easier navigation, as well as sidebar on the right. The awesome fact is that you can edit those too, "fixing" links to point to proper localized page for your language. It requires a bit hacking around, but it's possible.

For example, ASF **[home page](https://github.com/JustArchi/ArchiSteamFarm/wiki)** includes a text such as:

> If you're a new user, we recommend starting with **[setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** guide.

Which is originally written as:

```md
If you're a new user, we recommend starting with **[setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** guide.
```

You on the crowdin see instead:

> If you're a new user, we recommend starting with <0>setting up</0> guide.

Firstly, you translate it as usual, leaving everything in-tact. This would be example for Polish language:

> Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z <0>przewodnika po konfiguracji</0>.

Now, if the link is a generic link that points outside of the wiki (e.g. to latest ASF release), you can leave it as it is since you don't want to edit it. You can save it and move forward.

However, if the link **does** point further inside the wiki, like the one above, you can actually **remove** `<0>` tags and replace them with pure HTML appropriate for new location.

We have a `https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up` link, so our Polish version is `https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-pl-PL`. We can now hover over `<0>` tag to see how it's defined, usually it'll be either `<strong><a></a></strong>` or `<a></a>` alone. Then, with knowledge of what `<0>` is in fact replaced with, we can rewrite it to point into new location:

> Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z <strong><a href="https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-pl-PL">przewodnika konfiguracji</a></strong>.

This will properly transform HTML back to markdown:

```md
Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika konfiguracji](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-pl-PL)**.
```

And finally into wiki text:

> Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika konfiguracji](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-pl-PL)**.

In similar way you can translate (and point to new locations) our sidebar! 🙂

---

Thank you for helping us to translate ASF! 👍 