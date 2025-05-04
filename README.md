**grotaclas Bugfixes** is a mod for the game Europa Universalis IV which fixes bugs in version 1.37.5.

Highlights include:
* Prevent janissary events from creating hundreds of unrest in Ottoman provinces
* Fix suez canal constructions from mission event if the country has inflation and an empty treasury
* Make sure that "A Pretender arises" always has an event option. This is [the oldest unfixed bug in the game](https://forum.paradoxplaza.com/forum/threads/possible-issue-with-event-purple_phoenix-1.775354/) and has existed since the release
* Fix inflation reduction when repaying loan with economic ideas(you still need to repay each loan separately to get the effect). This fixes the error log spam caused by this bug
* Add scrollbar to naval doctrines if there are more than 5 so that you can actually select them
* Fix wrong HRE membership after looking at other start dates
* Fix country renaming to "Seljuk Empire" when forming Rum as Aq/Qara Qoyunlu and "The Hanseatic League" when forming Lübeck
* Prevent Shwedagon Pagoda monument from getting destroyed by countries which don't own it
* Fix wrongly broken province names in the tutorial
* Fix burgundian inheritance for non-monarchies

For the full list of fixes and links to their patches and bugreports see [CHANGELOG](CHANGELOG.md). For a quick overview [CHANGELOG_summary](CHANGELOG_summary.md) contains one line for each bugfix. Each bugfix also has a patch in the [patches](patches) folder. These patches can be used by the eu4 developers to apply them to their files if they want to release a new version or by mod authors to fix these bugs in their mods if they override the vanilla files.
