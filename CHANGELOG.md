# 2025-05-04 (initial release)

## fix Hanlin Academy modifier for korean and japanese missions

bugreport: https://forum.paradoxplaza.com/forum/threads/event-keju-for-korea-not-giving-province-modifier.1713815/
Patch: [0001-fix-Hanlin-Academy-modifier-for-korean-and-japanese-missions](patches/0001-fix-Hanlin-Academy-modifier-for-korean-and-japanese-missions.patch)

## prevent event 728 "Claims on Our Rivals!" from firing with no valid province

there was a mismatch between the event trigger and
the conditions in the random_rival_country in the immediate
section which selected the event target claims_province

In the case that the only bordering rival province is
the capital of a country which is not an OPM, it would
save no country and the event text would use the root country
instead.

There is no bugreport, but the following reddit post shows the bug in
action:
https://www.reddit.com/r/eu4/comments/1gxmox6/orleans_rival_of_orleans/
Patch: [0002-prevent-event-728-Claims-on-Our-Rivals-from-firing-with-no-valid-province](patches/0002-prevent-event-728-Claims-on-Our-Rivals-from-firing-with-no-valid-province.patch)

## align conditions for "Fate of the Mandate" with its tooltip

The tooltip says that you need the highest dev and biggest army in
china, but the actual conditions checked if there is a country in china
which has less dev and army. So you could complete it with the second
lowest dev and army, but you could not complete it if you were the only
country in china

No bugreport, but posted on reddit: https://www.reddit.com/r/eu4/comments/1gxyugc/what_do_you_mean_im_not_the_highest_developed/
Patch: [0003-align-conditions-for-Fate-of-the-Mandate-with-its-tooltip](patches/0003-align-conditions-for-Fate-of-the-Mandate-with-its-tooltip.patch)

## allow mission "Fate of the Mandate"(fate_of_the_mandate) to be completed by dismantling the mandate

no bugreport, but related to EUIV-35600 which is about another mission
which has the same problem
Patch: [0004-allow-mission-Fate-of-the-Mandatefate_of_the_mandate-to-be-completed-by-dismantling-the-mandate](patches/0004-allow-mission-Fate-of-the-Mandatefate_of_the_mandate-to-be-completed-by-dismantling-the-mandate.patch)

## partial fix for hussite incident when emperor changes

This is related to EUIV-33333, but contrary to what was said in the
comments of that report, the code is not actually written in a way
which requires the emperor both have -50% warscore and be an OPM. The
reason why it is not working in the save of that bugreport is because
the emperor changed and the previous emperor is not an OPM. But the
previous emperor is the ROOT scope in the incident. This patch fixes the
scope, so that the code works as it is written, but it doesn't fix that
the code is not aligned with the new tooltip. And the warscore condition
is bugged as well, because checking for warscore in is_in_war seems to
not work in some cases. EUIV-35830 is another example of that
Patch: [0005-partial-fix-for-hussite-incident-when-emperor-changes](patches/0005-partial-fix-for-hussite-incident-when-emperor-changes.patch)

## make confucian monuments work with harmonized religious groups

they used has_owner_harmonized_religion = yes, but that only works for
eastern religions which are harmonized individually. It doesn't work
if the province has a religion whose group was harmonized. Ideally a
bugfix would change the behavior of has_owner_harmonized_religion, but
that would need a code change, so this fix uses the following workaround
instead:

owner = {
	has_harmonized_with = root
}

Bugreport is at https://forum.paradoxplaza.com/forum/threads/temple-of-confucius-monument-bug.1725911/
Patch: [0006-make-confucian-monuments-work-with-harmonized-religious-groups](patches/0006-make-confucian-monuments-work-with-harmonized-religious-groups.patch)

## Dont remove zoroastrian_prophet_in_the_court when firing different advisor

The modifier gets set by the event flavor_per.69 and is supposed to last
"until the recruited advisor is no longer in our service".
But it also got removed when another advisor type gets removed

Bugreport: https://forum.paradoxplaza.com/forum/threads/persia-loses-modifier-the-neo-zoroastrian-prophet-before-the-corresponding-advisor-dies.1726069/
Patch: [0007-Dont-remove-zoroastrian_prophet_in_the_court-when-firing-different-advisor](patches/0007-Dont-remove-zoroastrian_prophet_in_the_court-when-firing-different-advisor.patch)

## fix branching missions when forming a country while having branching buttons

the most common case is forming Persia as Hisn Kayfa, because Hisn Kayfa
has branching previews without allowing you to actually select the
branches with them. If you then form another country, the on_change_tag_effect
triggers update_mission_previews which triggers clear_mismatching_mission_keys_for_batch
for each batch which changes the variable can_preview_missions_var by -1
each time. If you have less than three active branches you end up
with a negative can_preview_missions_var which prevents future branching
buttons from working. This patch forces can_preview_missions_var to be 0
afterwards. This is not a perfect solution, because the variable should be
higher than 0 if there are still active previews, but this would require
a complete rework of these effects which has a higher risk of breaking
something else. This fix certainly doesn't break anything which wasn't
broken before and I don't think there are vanilla situations in which
you can have some mismatching previews while also having previews which
should still be displayed.

Bugreport: https://forum.paradoxplaza.com/forum/threads/buttons-for-persian-branching-mission-do-not-appear.1704811/
Patch: [0008-fix-branching-missions-when-forming-a-country-while-having-branching-buttons](patches/0008-fix-branching-missions-when-forming-a-country-while-having-branching-buttons.patch)

## increase manpower from boh_reformatio_sigismundi_hre

The modifier boh_reformatio_sigismundi_hre is one of the possible
rewards from the WoC mission boh_reformatio_sigismundi. Its
manpower_against_imperial_enemies = 0.2 literally gives 0.2 max manpower
while being at war with a non-HRE country. I guess this was meant to be
+20%, but the modifier doesn't work in this way. I choose 20000 as a
more reasonable amount, because it still starts with a 2 and the only
other mission which gives this modifier gives 35000. I think 20000 is
not too much, because the mission requires being emperor which already
gives a lot of manpower and it requires the 5th HRE reform.

No bugreport, but a reddit discussion: https://www.reddit.com/r/eu4/comments/1i4ben9/what_in_the_name_of_god_is_020_manpower_against/
Patch: [0009-increase-manpower-from-boh_reformatio_sigismundi_hre](patches/0009-increase-manpower-from-boh_reformatio_sigismundi_hre.patch)

## allow Amsterdam Bourse with roman culture

Bugreport: https://forum.paradoxplaza.com/forum/threads/amsterdam-bourse-does-not-fit-roman-culture.1698262/
Patch: [0010-allow-Amsterdam-Bourse-with-roman-culture](patches/0010-allow-Amsterdam-Bourse-with-roman-culture.patch)

## re-enable GC missions for VAL and CAT if DOM is active

The aragonese missions from Domination are only for Aragon, so Valencia
and Catalonia should keep access to the aragonese missions from Golden Century
which they have without Domination(as long as Golden Century is active)

Bugreport: https://forum.paradoxplaza.com/forum/threads/mission-bugs-report.1697792/
Patch: [0011-re-enable-GC-missions-for-VAL-and-CAT-if-DOM-is-active](patches/0011-re-enable-GC-missions-for-VAL-and-CAT-if-DOM-is-active.patch)

## turn MLO into a monarchy when FRA forms a PU over them

flavor_fra.109 turns Milan from a vassal into a junior partner without
checking if they are a monarchy which will break the union when they
elect a new ruler. This patch forces them to be a monarchy.

Bugreport: https://forum.paradoxplaza.com/forum/threads/playing-as-france-i-got-a-subjication-cassus-belli-through-an-event-when-the-amrosian-republic-fired-i-forced-them-to-become-my-vassal-through-war.1698229/
and https://forum.paradoxplaza.com/forum/threads/french-event-securing-milanese-lands-personl-union-option-does-not-change-milans-government.1694976/
Patch: [0012-turn-MLO-into-a-monarchy-when-FRA-forms-a-PU-over-them](patches/0012-turn-MLO-into-a-monarchy-when-FRA-forms-a-PU-over-them.patch)

## fix "Italian Coastal Influence" privilege

It gave local modifiers which don't work in the country scope

Bugreport: https://forum.paradoxplaza.com/forum/threads/the-privilege-of-trebizond-italian-coastal-influence-does-not-give-you-the-maluses-it-says-it-gives.1704860/
Patch: [0013-fix-Italian-Coastal-Influence-privilege](patches/0013-fix-Italian-Coastal-Influence-privilege.patch)

## Fix highlighting for Iroquois missions
Patch: [0014-Fix-highlighting-for-Iroquois-missions](patches/0014-Fix-highlighting-for-Iroquois-missions.patch)

## Allow 150+ dev free company for SWE Engelbrekt mission
Patch: [0015-Allow-150-dev-free-company-for-SWE-Engelbrekt-mission](patches/0015-Allow-150-dev-free-company-for-SWE-Engelbrekt-mission.patch)

## prevent shwedagon_pagoda from getting destroyed by countries which don't own it

china_events.44 could fire for any chinese/eastern country
if leviathan was active

Bugreports: https://forum.paradoxplaza.com/forum/threads/eu-iv-event-the-gilded-pagoda-can-fire-even-when-you-dont-own-the-province-that-the-great-project-is-in.1579493/
https://forum.paradoxplaza.com/forum/threads/eu-iv-shwedagon-pagoda-monument-downgrade.1560685/
Patch: [0016-prevent-shwedagon_pagoda-from-getting-destroyed-by-countries-which-dont-own-it](patches/0016-prevent-shwedagon_pagoda-from-getting-destroyed-by-countries-which-dont-own-it.patch)

## prevent Rotten Borough from assigning seat to territory

Bugreport: https://forum.paradoxplaza.com/forum/threads/rotten-boroughs-assigns-seat-to-territory.1624339
Patch: [0017-prevent-Rotten-Borough-from-assigning-seat-to-territory](patches/0017-prevent-Rotten-Borough-from-assigning-seat-to-territory.patch)

## Only allow fifth monarchists reform for christian custom nations

Non-christians who would select the reform would immediately lose it,
because the trigger is religion_group = christian

Bugreport: https://forum.paradoxplaza.com/forum/threads/custom-nations-fifth-monarchist-regime-should-require-catholic-religion-group-otherwise-you-go-bankrupt-on-day-1.1727299
Patch: [0018-Only-allow-fifth-monarchists-reform-for-christian-custom-nations](patches/0018-Only-allow-fifth-monarchists-reform-for-christian-custom-nations.patch)

## fix expand infrastructure effect for Expand the Bogue missions

The following missions had the problem that they had the hidden effect
to set a flag which was supposed to increase merchant loyalty when
expanding infrastructure, but that flag was never used. But players
without Leviathan got the alternative effect of 100 dip:
 * mng_expand_bogue
 * mng_expand_bogue2
 * mng_expand_bogue3
 * yua_expand_bogue

This patch also moves the effect out of the hidden effects so that its
custom tooltip is shown. For the other effect of the mission, I changed
the tooltip from mng_pearly_estuary_upgraded_tt to yua_pearly_estuary_upgraded_tt
which was already in use in the mission yua_expand_bogue, because it
explains the actual effects instead of just saying that the estuary will
be upgraded

Bugreport: EUIV-35604 and https://forum.paradoxplaza.com/forum/threads/expand-the-bogue-eoc-mission-is-weird.1718898/
Patch: [0019-fix-expand-infrastructure-effect-for-Expand-the-Bogue-missions](patches/0019-fix-expand-infrastructure-effect-for-Expand-the-Bogue-missions.patch)

## fix or remove outdated dynamic names for the province Dortmund(69)

The province ID 69 used to be called Oberschwaben before 1.30 and
Konstanz before patch 1.12. In 1.30 the province 69 was moved from southern
Germany to northern Germany and renamed to Dortmund. Part of the old
location of province 69 is the new province Constance(4712) since 1.30.
I changed all dynamic names which call 69 a variation
of Constance/Konstanz to 4712 instead(if the culture didn't have a name
for 4712 already) and removed the entries which called 69 a variation of
Oberschwaben.

Bugreport: https://forum.paradoxplaza.com/forum/threads/konstanzs-dynamic-name-in-french-is-in-the-wrong-place.1728238/
Patch: [0020-fix-or-remove-outdated-dynamic-names-for-the-province-Dortmund69](patches/0020-fix-or-remove-outdated-dynamic-names-for-the-province-Dortmund69.patch)

## Allow son_modernize_army to be completed without CoC

the mission requires 80% army professionalism. 1.37.0 removed the
professionialism requirement from the previous mission
son_military_expertise if the Cradle of Civilization DLC is not active.
This patch applies the same fix to son_modernize_army
Patch: [0021-Allow-son_modernize_army-to-be-completed-without-CoC](patches/0021-Allow-son_modernize_army-to-be-completed-without-CoC.patch)

## fix tooltip son_own_develop_clothes_and_salt_ct

it said that you gain +25% global trade power, even though the modifier
gave +15% in all publicly available versions
Patch: [0022-fix-tooltip-son_own_develop_clothes_and_salt_ct](patches/0022-fix-tooltip-son_own_develop_clothes_and_salt_ct.patch)

## Fix encoding in the tutorial

The files were encoded in utf8 instead of windows-1252/cp1252 since
patch 1.37.0. But the files were not properly converted to utf8. Instead
all non-ASCII characters were replaced by the utf8 replacement
character(U+FFFD ; looks like a questionmark in a box) which makes it
impossible to just convert the files back.

So this fix was done by taking the 1.36.2 tutorial saves and applying the
1.37.0 changes which were in lines without the utf8 replacement character. But I might
have missed 1.37.0 changes if they were in lines with that character

Bugreport: EUIV-33125
Patch: [0023-Fix-encoding-in-the-tutorial](patches/0023-Fix-encoding-in-the-tutorial.patch)

## Fix construction of Suez canal with inflation in event flavor_mam.115

The event uses add_great_project with instant = no which requires that
you actually have the money to start the construction. The event
achieves this by giving the country 20k ducats and removing the money
again afterwards. But the canal costs more than 20k if there are
increased construction costs(e.g. from inflation). Then the construction
is not started if the treasury does not have enough money to cover the
difference.

This fix just gives and removes 100k ducats instead of 20k. This should
cover all reasonable cost increases.

Bugreport: https://forum.paradoxplaza.com/forum/threads/mamluks-construction-commences-in-sharqiya-event-no-longer-builds-suez-canal.1675359/
Patch: [0024-Fix-construction-of-Suez-canal-with-inflation-in-event-flavor_mam115](patches/0024-Fix-construction-of-Suez-canal-with-inflation-in-event-flavor_mam115.patch)

## add missing traits to num_of_ruler_traits

Bugreport: https://forum.paradoxplaza.com/forum/threads/num_of_ruler_traits-not-having-humane_personality.1730551/
Patch: [0025-add-missing-traits-to-num_of_ruler_traits](patches/0025-add-missing-traits-to-num_of_ruler_traits.patch)

## let LAE keep BYZ missions if they were created by HAB

The Austrian mission hab_eastern_roman_empire turns their subject
Byzantium into the Latin Empire. It does not change their missions, so
they keep the Byzantium missions, but if they then try to select a
mission branch, their mission get reset to the generic missions. Other
ways in which Byzantium can form the Latin Empire prevent this by
setting the flag has_byz_missions. This patch adds the effect to set
the flag to the mission hab_eastern_roman_empire

No bugreport, but a reddit post: https://www.reddit.com/r/eu4/comments/1j7fr9z/my_vassal_the_latin_empire_suddenly_only_has/
Patch: [0026-let-LAE-keep-BYZ-missions-if-they-were-created-by-HAB](patches/0026-let-LAE-keep-BYZ-missions-if-they-were-created-by-HAB.patch)

## fix highlighting in mission Korean Self-Reliance (kor_achieve_juche)

no bugreport, but discussed in https://www.reddit.com/r/eu4/comments/1jmlksv/korean_selfreliance_not_firing/
Patch: [0027-fix-highlighting-in-mission-Korean-Self-Reliance-kor_achieve_juche](patches/0027-fix-highlighting-in-mission-Korean-Self-Reliance-kor_achieve_juche.patch)

## allow custom nations to keep livonian reforms

it already worked for most of them, but Livonian Absolute Monarchy and
Livonian Enlightened Monarchy only had the custom nation condition in
the potential and not in the trigger

bugreport: https://forum.paradoxplaza.com/forum/threads/multiple-livonian-monarchy-forms-get-removed-and-locked-for-custom-nations.1734968/
Patch: [0028-allow-custom-nations-to-keep-livonian-reforms](patches/0028-allow-custom-nations-to-keep-livonian-reforms.patch)

## fix patriarch authority in swedish events/decision

The swedish events "The Support of the Estates"(flavor_swe.108) and
"The Consequences of the Bloodbath of
[Root.Capital.GetName]"(flavor_swe.1080) gave 2000% patriarch authority
if Sweden was orthodox and the decision "Repay Debt to the
Clergy"(swe_repay_estate_church) required 2500% patriarch authority to
remove the modifier from the first event. This patch reduces the gain to
20% which I think was the intended value and the cost to 10% to match
the values for the other religions where the cost is half the gain.

No bugreport, but a reddit post: https://www.reddit.com/r/eu4/comments/1jrkw53/that_might_be_a_bit_difficult/
Patch: [0029-fix-patriarch-authority-in-swedish-eventsdecision](patches/0029-fix-patriarch-authority-in-swedish-eventsdecision.patch)

## avoid permaclaims on sea tiles from GER and JAP missions

The missions jap_conquer_south_china (Conquer South China) and
JHC_GER_scramble_for_africa (Scramble for Africa) use
every_trade_node_member_province to award permanent claims
without checking if the member province is a sea tile

no bugreport, but a reddit post: https://www.reddit.com/r/eu4/comments/1jo4w60/i_think_the_german_mission_tree_gave_me_claims_on
Patch: [0030-avoid-permaclaims-on-sea-tiles-from-GER-and-JAP-missions](patches/0030-avoid-permaclaims-on-sea-tiles-from-GER-and-JAP-missions.patch)

## remove extra spaces before regal numbers

Bugreport: https://forum.paradoxplaza.com/forum/threads/new-rulers-of-montferrat-have-an-extra-space-in-their-naming-before-their-regnal-number.1735208/
Patch: [0031-remove-extra-spaces-before-regal-numbers](patches/0031-remove-extra-spaces-before-regal-numbers.patch)

## Allow France to annex normal vassals without seizing land

Bugreport: https://forum.paradoxplaza.com/forum/threads/annexing-french-vassal-require-seize-land.1735199/
Patch: [0032-Allow-France-to-annex-normal-vassals-without-seizing-land](patches/0032-Allow-France-to-annex-normal-vassals-without-seizing-land.patch)

## Fix reconquest war against Kalmyks if the owner doesnt have full cores

When the event "The Arrival of the Kalmyks"(flavor_klm.1) releases the Kalmyk,
the previous owner loses their territorial core which breaks the
reconquest CB from the second event option. This fix adds the
territorial cores back before declaring the war. It also adds a
territorial core on the wargoal 2417 if the owner doesn't have a core on
it.

Bugreport: https://forum.paradoxplaza.com/forum/threads/the-kalmyk-error-and-the-problems-of-building-great-projects.1734253/
Patch: [0033-Fix-reconquest-war-against-Kalmyks-if-the-owner-doesnt-have-full-cores](patches/0033-Fix-reconquest-war-against-Kalmyks-if-the-owner-doesnt-have-full-cores.patch)

## Fix burgundian inheritance for non-monarchies

1.37.2 allowed non-monarchies with a royal marriage to be chosen as
bur_strongest_ally, but the event option "[bur_strongest_ally.GetName]
will defend us again."(incidents_bur_inheritance.1.d) is only available
if there also is a monarchy which fulfills the conditions for
bur_strongest_ally. This fix changes the trigger for the event option to
just check if bur_strongest_ally is set and is independent.

Bugreport: https://forum.paradoxplaza.com/forum/threads/burgundian-inheritance-is-still-restricted-to-monarchies-in-1-37-2.1688983/
Patch: [0034-Fix-burgundian-inheritance-for-non-monarchies](patches/0034-Fix-burgundian-inheritance-for-non-monarchies.patch)

## fix "Seljuk Empire" name when forming Rum as Aq and Qara Qoyunlu

It was bugged since the on_change_tag_effect was moved to the bottom
of the effects in version 1.37.3, because that effect undoes the name change.

This patch moves the name change below on_change_tag_effect

Bugreport https://forum.paradoxplaza.com/forum/threads/aq-qoyunlu-seljuk-empire-name-change-bugged.1707994/
Patch: [0035-fix-Seljuk-Empire-name-when-forming-Rum-as-Aq-and-Qara-Qoyunlu](patches/0035-fix-Seljuk-Empire-name-when-forming-Rum-as-Aq-and-Qara-Qoyunlu.patch)

## update event insight for flavor_ARB.5 in YEM_zaidi_caliphate

The event effect was changed in 1.37.0, but the insight was not updated.

Bugreport about the change of the event effect: https://forum.paradoxplaza.com/forum/threads/minor-bug-for-arabian-event-a-new-caliphate-caliphal-ambitions-mission.1620047/
Patch: [0036-update-event-insight-for-flavor_ARB5-in-YEM_zaidi_caliphate](patches/0036-update-event-insight-for-flavor_ARB5-in-YEM_zaidi_caliphate.patch)

## fix portrait of female Indian master recruiter

Bugreport: https://forum.paradoxplaza.com/forum/threads/wrong-advisor-portraits.1732523/
Patch: [0037-fix-portrait-of-female-Indian-master-recruiter](patches/0037-fix-portrait-of-female-Indian-master-recruiter.patch)

## use mesoamerican advisor portraits for new cultures

The cultures which have been added in mesoamerica in version 1.28 were
not assigned the mesoamerican advisor portraits. I assigned them for all
cultures except miskito, because miskito is in the chibchan culture
group which is shared with south american countries

Bugreport: https://forum.paradoxplaza.com/forum/threads/wrong-advisor-portraits.1732523/
Patch: [0038-use-mesoamerican-advisor-portraits-for-new-cultures](patches/0038-use-mesoamerican-advisor-portraits-for-new-cultures.patch)

## prevent incan event "Policies of Huascar" after the end of the civil war

Bugreport: https://forum.paradoxplaza.com/forum/threads/incan-civil-war-event-policies-of-leader-name-has-incorrect-name-in-event-text.1703452
Patch: [0039-prevent-incan-event-Policies-of-Huascar-after-the-end-of-the-civil-war](patches/0039-prevent-incan-event-Policies-of-Huascar-after-the-end-of-the-civil-war.patch)

## Prevent burghers agenda "Expand our Colony in x"(estate_burghers_colonial_nation) for countries which can't spawn CNs

Bugreport: https://forum.paradoxplaza.com/forum/threads/native-nations-in-the-new-world-get-an-unintended-diet-agenda.1702921/
Patch: [0040-Prevent-burghers-agenda-Expand-our-Colony-in-xestate_burghers_colonial_nation-for-countries-which-cant-spawn-CNs](patches/0040-Prevent-burghers-agenda-Expand-our-Colony-in-xestate_burghers_colonial_nation-for-countries-which-cant-spawn-CNs.patch)

## Prevent eunuchs agenda "Reform the Government" if the examination system reform is not available

No bugreport, but reddit post: https://www.reddit.com/r/eu4/comments/1k56z5h/eunuchs_want_a_govt_reform_that_i_dont_have/
Patch: [0041-Prevent-eunuchs-agenda-Reform-the-Government-if-the-examination-system-reform-is-not-available](patches/0041-Prevent-eunuchs-agenda-Reform-the-Government-if-the-examination-system-reform-is-not-available.patch)

## Align requirements of Eagle of Lübeck mission with tooltip and add fallback if there are no valid rivals

The tooltip hsa_largest_fleet_from_rivals_or_naval_ideas_tt says "Has a
larger fleet than any §Yrival§! country", but it doesn't check rivals.
Instead it checks countries which have rivaled Lübeck. This fix checks
both and adds the option to complete the mission by having the
biggest navy in the world.

No bugreport, but reddit post: https://www.reddit.com/r/eu4/comments/1k0ufw2/why_cant_i_complete_eagle_of_lubeck_mission/
Patch: [0042-Align-requirements-of-Eagle-of-Lbeck-mission-with-tooltip-and-add-fallback-if-there-are-no-valid-rivals](patches/0042-Align-requirements-of-Eagle-of-Lbeck-mission-with-tooltip-and-add-fallback-if-there-are-no-valid-rivals.patch)

## give japan claims on korea if DOM is active

The claims are needed for one of their starting missions. The patchnotes
for 1.37 already said "Forming Japan will now grant claims on Korea if
the Domination DLC is active.", but this was never implemented.

Bugreport: https://forum.paradoxplaza.com/forum/threads/forming-japan-does-not-grant-claims-on-korea-if-the-domination-dlc-is-active.1702742/
Patch: [0043-give-japan-claims-on-korea-if-DOM-is-active](patches/0043-give-japan-claims-on-korea-if-DOM-is-active.patch)

## Do not remove estuary/granary modifiers from flavor_mam.102 if the provinces get conquered

These modifiers replace the default modifiers nile_estuary_modifier or
granary_of_the_mediterranean, which are permanent. The technology in eu4
is not advanced enough to stop the nile from flowing into the sea, so
the estuary modifier can't just be removed. Bringing the original
modifier back would require changes in some on_actions, so I think it is
simpler and clearer to make the new modifiers permanent instead.

Bugreport: https://forum.paradoxplaza.com/forum/threads/expanded-granary-of-the-mediterranean-disappears-when-provinces-change-hands.1701042/
Patch: [0044-Do-not-remove-estuarygranary-modifiers-from-flavor_mam102-if-the-provinces-get-conquered](patches/0044-Do-not-remove-estuarygranary-modifiers-from-flavor_mam102-if-the-provinces-get-conquered.patch)

## Allow anglican and AVE to trigger event "Huguenot Rebels Need Our Help"(tyw_events.12)

Bugreport: https://forum.paradoxplaza.com/forum/threads/a-thirty-years-war-event-does-not-consider-anglicanism.1698943/
Patch: [0045-Allow-anglican-and-AVE-to-trigger-event-Huguenot-Rebels-Need-Our-Helptyw_events12](patches/0045-Allow-anglican-and-AVE-to-trigger-event-Huguenot-Rebels-Need-Our-Helptyw_events12.patch)

## Apply reward from fra_london even if FRA changes tag

Bugreport: https://forum.paradoxplaza.com/forum/threads/the-french-mission-effect-of-london-tower-disappears-when-it-forms-another-tag.1698790/
Patch: [0046-Apply-reward-from-fra_london-even-if-FRA-changes-tag](patches/0046-Apply-reward-from-fra_london-even-if-FRA-changes-tag.patch)

## Require fur province in the new world for "Natives Not Assisting"

Bugreport: https://forum.paradoxplaza.com/forum/threads/natives-not-assisting-firing-for-non-colonial-nations.1696527/
Patch: [0047-Require-fur-province-in-the-new-world-for-Natives-Not-Assisting](patches/0047-Require-fur-province-in-the-new-world-for-Natives-Not-Assisting.patch)

## Make Persian mission "Checks and Balance" work with all estates

The mission didn't consider dhimmi, brahmins, rajput, vaisyas, jains,
janissaries and eunuchs when dermining if all estates have 40 influence
and two estates have 60 influence.

Bugreport: https://forum.paradoxplaza.com/forum/threads/persian-mission-requirements-met-but-not-completable.1694807/
Patch: [0048-Make-Persian-mission-Checks-and-Balance-work-with-all-estates](patches/0048-Make-Persian-mission-Checks-and-Balance-work-with-all-estates.patch)

## Apply and remove DotF bonus correctly for Head of the Patriarchate

The reform "Head of the Patriarchate"(head_of_the_patriarchy_reform)
uses improved_defender_of_faith = yes, but in contrast to the other
reforms which have that attribute, it didn't add the modifier if the
country was already the defender of the faith when they enact the reform
and it didn't remove the modifier if the reform was changed

Bugreport: https://forum.paradoxplaza.com/forum/threads/russia-fate-of-the-peasantry-mission-head-of-the-patriarchate-reform-bugs.1674468/
Patch: [0049-Apply-and-remove-DotF-bonus-correctly-for-Head-of-the-Patriarchate](patches/0049-Apply-and-remove-DotF-bonus-correctly-for-Head-of-the-Patriarchate.patch)

## fix Portuguese missions in RNW games

some missions which were not related to america were in slots which were
locked behind is_random_new_world = no and some of the missions which
were still available were not completable, because they needed these
missions

Bugreport: https://forum.paradoxplaza.com/forum/threads/eu-iv-portuguese-mission-tree-links-are-broken-in-random-new-world.1583931/
https://forum.paradoxplaza.com/forum/threads/portuguese-mission-are-missing.1686041/
Patch: [0050-fix-Portuguese-missions-in-RNW-games](patches/0050-fix-Portuguese-missions-in-RNW-games.patch)

## Fix wrong HRE membership after looking at other start dates

The problem was that some provinces have "hre = no" lines in their
history files on dates where they are not part of the HRE which causes
eu4 to add the provinces if you go backwards over that date
Patch: [0051-Fix-wrong-HRE-membership-after-looking-at-other-start-dates](patches/0051-Fix-wrong-HRE-membership-after-looking-at-other-start-dates.patch)

## Fix highlight for prussian_nation_general decision
Patch: [0052-Fix-highlight-for-prussian_nation_general-decision](patches/0052-Fix-highlight-for-prussian_nation_general-decision.patch)

## Fix temple bonus when switching between reforms

Enacting a reform which has temples_modifier = yes, gives the modifier
gov_expanded_temple_rights_mod to all provinces with a tax building and
removing the reform removes the modifier again. But when switching
between two reforms which have that attribute, it seems that the effect
of the new reform happens before the post_removed_effect/removed_effect
of the removed reform, so the modifier gets removed from all provinces.
This patch fixes it by only removing the modifier if the country does
not have the temples_modifier = yes anymore.

Bugreport: https://forum.paradoxplaza.com/forum/threads/russia-fate-of-the-peasantry-mission-head-of-the-patriarchate-reform-bugs.1674468
Patch: [0053-Fix-temple-bonus-when-switching-between-reforms](patches/0053-Fix-temple-bonus-when-switching-between-reforms.patch)

## add scrollbar to naval doctrines if there are more than 5
Patch: [0054-add-scrollbar-to-naval-doctrines-if-there-are-more-than-5](patches/0054-add-scrollbar-to-naval-doctrines-if-there-are-more-than-5.patch)

## Make it more likely that an emperor who has a negative opinion of Burgundy bans their entry in the incident Burgundy and the Empire

the factor was already there, but it wasn't working, because the emperor
checked his opinion of himself

Bugreport: https://forum.paradoxplaza.com/forum/threads/burgundy-and-the-empire-imperial-incident-issue-in-calculation-of-ai-factor.1731899/
Patch: [0055-Make-it-more-likely-that-an-emperor-who-has-a-negative-opinion-of-Burgundy-bans-their-entry-in-the-incident-Burgundy-and-the-Empire](patches/0055-Make-it-more-likely-that-an-emperor-who-has-a-negative-opinion-of-Burgundy-bans-their-entry-in-the-incident-Burgundy-and-the-Empire.patch)

## Do not remove "Expanded Daugava Estuary" modifier from Riga if province changes hands

The modifier gets added by the polish mission The Livonian
Lands(plc_break_livonia), and replaces the permanent modifier "Daugava
Estuary"(daugava_estuary_modifier), so it should also be permanent. This
is especially important, because the mission can be completed if the
province is owned by a subject and then the modifier would be lost when
the subject gets integrated.

Bugreport: https://forum.paradoxplaza.com/forum/threads/expanded-daugava-estuary-disappears-completely-after-annexing-vassal.1729695/
Patch: [0056-Do-not-remove-Expanded-Daugava-Estuary-modifier-from-Riga-if-province-changes-hands](patches/0056-Do-not-remove-Expanded-Daugava-Estuary-modifier-from-Riga-if-province-changes-hands.patch)

## Also allow "Expanded Daugava Estuary" modifier in all triggers which check for the normal modifier

Especially has_river_estuary_trigger and has_no_river_estuary_trigger
Patch: [0057-Also-allow-Expanded-Daugava-Estuary-modifier-in-all-triggers-which-check-for-the-normal-modifier](patches/0057-Also-allow-Expanded-Daugava-Estuary-modifier-in-all-triggers-which-check-for-the-normal-modifier.patch)

## Partially fix inflation reduction when repaying loan with economic ideas

It works now when repaying one loan, but repaying multiple loans with
the "repay multiple loan(s)" button will only reduce inflation once.
I don't see a way to change that via script, because on_loan_repaid gets
only called once. So the player has to pay back each loan separately to
reduce inflation for each loan

This also fixes the error.log messages:
    Script error! Script Object Token: "economic_ideas", Error: Trigger condition set on the wrong scope

Bugreport: https://forum.paradoxplaza.com/forum/threads/0-05-inflation-on-loan-repayment-with-4th-eco-idea-does-not-work.1725766/
Patch: [0058-Partially-fix-inflation-reduction-when-repaying-loan-with-economic-ideas](patches/0058-Partially-fix-inflation-reduction-when-repaying-loan-with-economic-ideas.patch)

## Make sure that "A Pretender arises" always has an event option

The only event option in "A Pretender arises"(purple_phoenix.1)
had the trigger that legitimacy must be less than 50, but the event can
also happen if the country has a regency. From the localisation it looks
like there was supposed to be a second option "Support the Pretender"(purple_phoenix.1.b),
which was never implemented. As I don't know what it is supposed to be,
I went with the safe choice and removed the condition from the first
option, so that it is always available.

I think this is the oldest unfixed bug in the game. The bugreport is
from 2014 and the event code is unchanged since the release of the game.

Bugreport: https://forum.paradoxplaza.com/forum/threads/possible-issue-with-event-purple_phoenix-1.775354/
Patch: [0059-Make-sure-that-A-Pretender-arises-always-has-an-event-option](patches/0059-Make-sure-that-A-Pretender-arises-always-has-an-event-option.patch)

## fix highlighting in Ottoman base game mission Conquer Greece(conquer_southern_greece)

It didn't highlight provinces owned by subjects even though the mission
requires that they are owned directly
Patch: [0060-fix-highlighting-in-Ottoman-base-game-mission-Conquer-Greececonquer_southern_greece](patches/0060-fix-highlighting-in-Ottoman-base-game-mission-Conquer-Greececonquer_southern_greece.patch)

## fix highlighting in City of the World's Desire(defeat_the_byzantine_empire)

It didn't highlight provinces owned by subjects even though the mission
requires that they are owned directly
Patch: [0061-fix-highlighting-in-City-of-the-Worlds-Desiredefeat_the_byzantine_empire](patches/0061-fix-highlighting-in-City-of-the-Worlds-Desiredefeat_the_byzantine_empire.patch)

## Fix highlighting in "St. Olav Tower"(fin_neva)
Patch: [0062-Fix-highlighting-in-St-Olav-Towerfin_neva](patches/0062-Fix-highlighting-in-St-Olav-Towerfin_neva.patch)

## Fix reward when completing Defense in Depth(BYZ_defence_in_depth) with level 3 "Theodosian Walls"

Bugreport: https://forum.paradoxplaza.com/forum/threads/defense-in-depth-mission-borked-again-or-maybe-it-was-never-quite-unborked.1724366/
Patch: [0063-Fix-reward-when-completing-Defense-in-DepthBYZ_defence_in_depth-with-level-3-Theodosian-Walls](patches/0063-Fix-reward-when-completing-Defense-in-DepthBYZ_defence_in_depth-with-level-3-Theodosian-Walls.patch)

## Make reform Ganden Phodrang available to Sino-Tibetan countries

Bugreport: https://forum.paradoxplaza.com/forum/threads/sinicizing-culture-removes-unique-tibet-government-reform.1724362/
Patch: [0064-Make-reform-Ganden-Phodrang-available-to-Sino-Tibetan-countries](patches/0064-Make-reform-Ganden-Phodrang-available-to-Sino-Tibetan-countries.patch)

## Fix devastation trigger and highlighting for "Fortify the Coast" missions

The devastation trigger for the Ming version(mng_fortify_coast) was fixed
to only check coastal provinces, but this change was not applied to the
japanese(mng_fortify_coast2) and korean versions(mng_fortify_coast3).

The highlighting didn't work properly for any case. The Ming version
highlighted provinces which had no devastation and which didn't have a
coastal defense building or 20 dev, even if the country already had 8
provinces to fulfill that part of the condition. This made it hard for
players to find the provinces with devastation. The japanese and korean
versions stopped highlighting when the province had either a coastal
defense or 20 dev and it didn't highlight devastation at all

Bugreport: https://forum.paradoxplaza.com/forum/threads/eu-iv-japanese-mandate-of-heaven-fortify-the-coast-mission-checks-incorrect-provinces-for-devastation.1600486/
Patch: [0065-Fix-devastation-trigger-and-highlighting-for-Fortify-the-Coast-missions](patches/0065-Fix-devastation-trigger-and-highlighting-for-Fortify-the-Coast-missions.patch)

## Bring tooltips of "Fortify the Coast" missions more in line with the effects

It is still a mess, because they mention a Ming mission in the Korean
and Japanese version and they don't mention that the province modifier
requires 20 dev
Patch: [0066-Bring-tooltips-of-Fortify-the-Coast-missions-more-in-line-with-the-effects](patches/0066-Bring-tooltips-of-Fortify-the-Coast-missions-more-in-line-with-the-effects.patch)

## Cede goa to the correct nation in the event "Vasco da Gama in India"

Both the golden century mission gc_portugal_discovers_india and the
base game mission portugal_discovers_india trigger the event "Vasco da
Gama in India", which cedes Goa to Portugal, even if country which
completed the mission is not Portugal anymore(e.g. because Portugal
formed Brazil or the Roman Empire which don't give new missions)

Bugreport: https://forum.paradoxplaza.com/forum/threads/portuguese-mission-tree-gives-goa-to-the-wrong-nation.1692350/
Patch: [0067-Cede-goa-to-the-correct-nation-in-the-event-Vasco-da-Gama-in-India](patches/0067-Cede-goa-to-the-correct-nation-in-the-event-Vasco-da-Gama-in-India.patch)

## Allow Portuguese missions to be completed after forming another country

When forming another country as Portugal which does not give new
missions, all missions should be completable, but the missions
"Push to India"(gc_portugal_discovers_india, portugal_discovers_india)
 and "Secure Trade"(gc_por_indonesia_dominance, por_indonesia_dominance)
had POR in their triggers, so it wasn't possible to complete them(unless
Portugal also happened to fulfill the conditions)

Bugreport: https://forum.paradoxplaza.com/forum/threads/eu-iv-portuguese-missions-as-non-portugal-are-bugged.1426030/
Patch: [0068-Allow-Portuguese-missions-to-be-completed-after-forming-another-country](patches/0068-Allow-Portuguese-missions-to-be-completed-after-forming-another-country.patch)

## Reduce amount of land gain for successful eunuch rebels from 100% to 10%

Bugreport: https://forum.paradoxplaza.com/forum/threads/eunuch-rebel-success-gives-100-crownland-to-eunuchs.1688585/
Patch: [0069-Reduce-amount-of-land-gain-for-successful-eunuch-rebels-from-100-to-10](patches/0069-Reduce-amount-of-land-gain-for-successful-eunuch-rebels-from-100-to-10.patch)

## Fix partiarch authority gain when selling titles while having the reform "Superiority of the State" and "Grant Land to the Monasteries"

Bugreport: https://forum.paradoxplaza.com/forum/threads/no-patriarch-authority-gain-with-superiority-of-the-state-government-reform-when-selling-titles.1688572/
Patch: [0070-Fix-partiarch-authority-gain-when-selling-titles-while-having-the-reform-Superiority-of-the-State-and-Grant-Land-to-the-Monasteries](patches/0070-Fix-partiarch-authority-gain-when-selling-titles-while-having-the-reform-Superiority-of-the-State-and-Grant-Land-to-the-Monasteries.patch)

## Prevent Hawai'i and Viti formation in random nation games

No bugreport, but mentioned on reddit: https://www.reddit.com/r/eu4/comments/1kcsmpg/why_does_hawaii_care_about_beljamen/
Patch: [0071-Prevent-Hawaii-and-Viti-formation-in-random-nation-games](patches/0071-Prevent-Hawaii-and-Viti-formation-in-random-nation-games.patch)

## Make "Constitutional Restoration" event give alternative reforms for countries which can't have the Parliamentarism reform

"Restore the Senate" for Byzantium
"A Revolutionary Council" for revolutionary countries with emperor DLC.
Without the DLC, revolutionary countries don't have a parliament reform,
so the event is now blocked for them.

The event now also makes sure that you have a high enough reform tier to
for the reform which you get

Bugreport: https://forum.paradoxplaza.com/forum/threads/repeatedly-getting-the-constitutional-restoration-event-despite-being-revolutionary-empire.1615990/
Patch: [0072-Make-Constitutional-Restoration-event-give-alternative-reforms-for-countries-which-cant-have-the-Parliamentarism-reform](patches/0072-Make-Constitutional-Restoration-event-give-alternative-reforms-for-countries-which-cant-have-the-Parliamentarism-reform.patch)

## Prevent event "A Zoroastrian Identity" during war

The event "A Zoroastrian Identity"(flavor_per.66) changes the tag to
Eranshahr, which causes bugs with war participation. Blocking the event
from happening during war is a partial fix, because the event could
happen before the war, but the player could click on the event option
after the war started.

Bugreport: https://forum.paradoxplaza.com/forum/threads/forming-eranshar-during-a-war-does-not-keep-trace-of-your-war-participation.1615632/
Patch: [0073-Prevent-event-A-Zoroastrian-Identity-during-war](patches/0073-Prevent-event-A-Zoroastrian-Identity-during-war.patch)

## Custom militarization idea now also gives early/late militarization

Normally custom nations can only have prussian_monarchy_base which uses
monthly_militarized_society, but when forming Prussia, the country gets
access to prussian_monarchy which uses
monthly_prussian_militarized_society_1,
monthly_prussian_militarized_society_2, and
monthly_prussian_militarized_society_3. This patch adds these modifiers
to the custom idea custom_monthly_militarized_society

Bugreport: https://forum.paradoxplaza.com/forum/threads/custom-nations-militarization-idea-doesnt-work-when-reforming-into-prussia.1613965/
Patch: [0074-Custom-militarization-idea-now-also-gives-earlylate-militarization](patches/0074-Custom-militarization-idea-now-also-gives-earlylate-militarization.patch)

## Give countries with Margravate reform another reform if Domination isn't active

This affects Baden, Brandenburg and Montferrat

Bugreport: https://forum.paradoxplaza.com/forum/threads/eu-iv-without-domination-montferrat-starts-without-tier-1-government-reform.1578649/
Patch: [0075-Give-countries-with-Margravate-reform-another-reform-if-Domination-isnt-active](patches/0075-Give-countries-with-Margravate-reform-another-reform-if-Domination-isnt-active.patch)

## Prevent high base unrest from janissary disaster

The unrest could go into the hundreds, because the Face the Janissaries
decisions add 10 base unrest to several provinces each time and
they can be enacted again if the country breaks to rebels. This can
happen every few months if the Ottomans are small.

This patch prevents the base unrest from the janissary events from going
above 15 per province and it changes the AI behavior so that they only
enact the decisions if they have at least 100k troops, so that they have
a chance of fighting the rebels which are at least 100k in total.

Bugreport: https://forum.paradoxplaza.com/forum/threads/eu-iv-absurdly-high-base-unrest.1597636/
https://forum.paradoxplaza.com/forum/threads/eu-iv-after-taking-provinces-from-ottomans-during-decadence-disaster-some-have-over-200-unrest-which-does-not-decay.1600952/
https://forum.paradoxplaza.com/forum/threads/eu-iv-base-unrest-200-in-certain-provinces-no-matter-the-conditions.1590988/
https://forum.paradoxplaza.com/forum/threads/eu-iv-ai-controlled-ottomans-in-janissary-rebels-doom-loop.1587523/
https://forum.paradoxplaza.com/forum/threads/eu-iv-conquered-ottoman-lands-with-endless-rebels.1586833
https://forum.paradoxplaza.com/forum/threads/ottoman-unrest.1614608
https://forum.paradoxplaza.com/forum/threads/unrest-stacked-to-the-moon.1720465/
Patch: [0076-Prevent-high-base-unrest-from-janissary-disaster](patches/0076-Prevent-high-base-unrest-from-janissary-disaster.patch)

## Apply absolutism impact from court ideas to all privileges

Some privileges were missing the 20% absolutism impact reduction from
the 5th court idea respected_authority. I also found two which wrongly
gave absolutism even though the privilege didn't reduce absolutism in
the first place.

Bugreport: https://forum.paradoxplaza.com/forum/threads/absolutism-malus-reduction-from-court-ideas-does-not-apply-for-some-estate-privileges.1717849/
Patch: [0077-Apply-absolutism-impact-from-court-ideas-to-all-privileges](patches/0077-Apply-absolutism-impact-from-court-ideas-to-all-privileges.patch)

## Add missing perma claims on muslim provinces to arabia mission event "Second Islamic Golden Age"

The tooltip ARB_permanent_claims_on_all_muslim_provinces, but the effect
was never implemented.

Bugreport: https://forum.paradoxplaza.com/forum/threads/final-arabia-mission-not-giving-permanent-claims.1717208/
Patch: [0078-Add-missing-perma-claims-on-muslim-provinces-to-arabia-mission-event-Second-Islamic-Golden-Age](patches/0078-Add-missing-perma-claims-on-muslim-provinces-to-arabia-mission-event-Second-Islamic-Golden-Age.patch)

## Fix "United Horn" modifier from horn of africa missions

It is a country modifier which gave local modifiers instead of global
modifiers.

Bugreport: https://forum.paradoxplaza.com/forum/threads/horn-of-africa-mission-unify-the-horn-of-africa-gives-wrong-modifier.1714187/
Patch: [0079-Fix-United-Horn-modifier-from-horn-of-africa-missions](patches/0079-Fix-United-Horn-modifier-from-horn-of-africa-missions.patch)

## Fix highlighting for Restore the Mongol Empire

The highlighting didn't reflect that all provinces can be owned by
non-tributary subjects since version 1.37.

Bugreport: https://forum.paradoxplaza.com/forum/threads/mongol-empire-decision-requirement-description-and-province-highlighting.1713440/
Patch: [0080-Fix-highlighting-for-Restore-the-Mongol-Empire](patches/0080-Fix-highlighting-for-Restore-the-Mongol-Empire.patch)

## Prevent seven provinces buff from being wrongly applied to conquered provinces of a different culture group

Bugreport: https://forum.paradoxplaza.com/forum/threads/high-altitude-adaptation-not-working-in-a-province.1677369/#post-29626760
Patch: [0081-Prevent-seven-provinces-buff-from-being-wrongly-applied-to-conquered-provinces-of-a-different-culture-group](patches/0081-Prevent-seven-provinces-buff-from-being-wrongly-applied-to-conquered-provinces-of-a-different-culture-group.patch)

## Fix renaming to "Lithuanian-Polish Commonwealth" when forming PLC as LIT

Bugreport: https://forum.paradoxplaza.com/forum/threads/forming-commonwealth-as-lithuania-does-not-change-the-name-to-lithuanian-polish-commonwealth.1711054/
Patch: [0082-Fix-renaming-to-Lithuanian-Polish-Commonwealth-when-forming-PLC-as-LIT](patches/0082-Fix-renaming-to-Lithuanian-Polish-Commonwealth-when-forming-PLC-as-LIT.patch)

## Fix renaming to "The Hanseatic League" when forming HSA

No bugreport, but reddit post: https://www.reddit.com/r/eu4/comments/1hzk1r7/why_did_my_name_became_l%C3%BCbeck_instead_of_the/
Patch: [0083-Fix-renaming-to-The-Hanseatic-League-when-forming-HSA](patches/0083-Fix-renaming-to-The-Hanseatic-League-when-forming-HSA.patch)

## Fix maritime idea reward from venetian mission "Masters of the Sea"

The reward was wrongly applied when having trade ideas instead of
maritime ideas.

Also prevent giving out the naval ideas reward even when not having
naval ideas.

And simplify the mission effect by moving the tooltips for the effects
out of the if/else blocks which are used for the yes/no tooltip lines

Bugreport: https://forum.paradoxplaza.com/forum/threads/venice-mission-doesnt-give-the-rewards-it-says-it-does.1709353/
Patch: [0084-Fix-maritime-idea-reward-from-venetian-mission-Masters-of-the-Sea](patches/0084-Fix-maritime-idea-reward-from-venetian-mission-Masters-of-the-Sea.patch)

## Disable privilege "Allow Religious Delegation" for the Papal States

The Papal States is not allowed to use the estate action which is the only real
effect of the privilege(besides 5% clergy influence), so it does not
make sense that they can enact the privilege.

Bugreport: https://forum.paradoxplaza.com/forum/threads/allowing-religious-delegation-as-papal-state-doesnt-actually-allow-religious-delegations.1707161/
Patch: [0085-Disable-privilege-Allow-Religious-Delegation-for-the-Papal-States](patches/0085-Disable-privilege-Allow-Religious-Delegation-for-the-Papal-States.patch)

## Fix second reward for Yuan mission "Reform Civil Registration"

The mission worked like this:

You get the first reward if you fulfill all the following conditions:

* granted land rights to eunuchs or doesn't have the eunuchs estate
* granted land rights to burghers or doesn't have the burghers estate
* granted land rights to nobles or doesn't have the nobles estate
* granted land rights to clergy or doesn't have the clergy estate
* granted land rights to nomadic tribes or doesn't have the nomadic tribes estate
* has at least one of the above estates

The second reward looks to be intended for the case that you don't get the first reward. So they put a `NOT = { }` around the first condition. But as cwtools correctly warns you if you do that, "Reminder: NOT does not mean NOT AND". So a `NOT` surrounding multiple conditions means

    NOT = {
        OR = {
            condition1
            condition2
            ...
        }
    }

So all the conditions must be false, so that the `NOT` becomes true and you get the second reward. This can be achieved (in theory) for the land rights conditions by having all those estates, but not granting land rights to any of them. But then the condition "has at least one of the above estates" is true. But if you have none of the estates, all the land rights conditions would be true, because of the "or doesn't have the estate" part. So it's impossible to get it.

This fix turns the NOT = {] into an NOT = { AND = {}}, so that you get
the second reward if you don't qualify for the first one.

No bugreport, but reddit post: https://www.reddit.com/r/eu4/comments/1kdzikx/how_do_i_get_the_alternative_reward_for_the/
Patch: [0086-Fix-second-reward-for-Yuan-mission-Reform-Civil-Registration](patches/0086-Fix-second-reward-for-Yuan-mission-Reform-Civil-Registration.patch)

# 2025-05-10

## make new ruler traits available to custom nations

No bugreport, but https://steamcommunity.com/app/236850/discussions/0/604155654554329049/
Patch: [0087-make-new-ruler-traits-available-to-custom-nations](patches/0087-make-new-ruler-traits-available-to-custom-nations.patch)

# 2025-05-20

## remove duplicate gov reform integrated_ottoman_officials

06_government_reforms_common.txt had two identical sections for the
reform
Patch: [0088-remove-duplicate-gov-reform-integrated_ottoman_officials](patches/0088-remove-duplicate-gov-reform-integrated_ottoman_officials.patch)

## make asha monarchy work for custom nations

Bugreport: https://forum.paradoxplaza.com/forum/threads/asha-monarchy-reform-becomes-invalid-after-being-chosen-by-a-custom-nation.1755596/
Patch: [0089-make-asha-monarchy-work-for-custom-nations](patches/0089-make-asha-monarchy-work-for-custom-nations.patch)

