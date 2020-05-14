# Roblox Influencer Program Unethical Videos
Generated May 14, 2020<br>
Report URL: https://github.com/TheBotAvenger/Roblox-Influencer-Program-Unethical-Videos/blob/master/report.md

## Purpose
The purpose of this report is to identify videos by Roblox's sponsored YouTubers (members
of the "Star" Creator Program) that do not align with Roblox's terms of use or laws in the
United States like the Child Online Privacy Protection Act (COPPA).

## Problem Statement
Roblox developers have been moderated against heavily based on the rules stated in the Roblox
Terms of Use. Roblox's sponsored content creators (aka "influencers") have regularly been breaking
the Roblox terms of use without any repercussions. These include but not limited to:
* Non-Giftcard Robux giveaways (Terms of Use 4(A), Community Rules 13)
* Collecting user data for Robux (Community Rules 2)
* Phishing / social engineering (Community Rules 2, 6, and 10)
* Gambling (Community Rules 10)
* Exploiting (Community Rules 12)
* Pornography (Community Rules 9)

The inconsistencies (and encouragement) of breaking these rules point to either the requirement to
change/remove the videos or change the terms of use on Roblox. Changing of the videos or the rules
can help Roblox's image and some of the social stigmas around Roblox's video creators by developers.

## Scope
The scope of this report covers YouTube videos due to the APIs and the ability to screen. Other services
(like twitch.tv and nimo.tv) are not searched for but can be noted. Searching will mostly include keywords
in the description or title due to the time required to sort the content, but manual entries can be added
if they come up. Some creators do create content that will not show up based on the keywords.

### Regarding Old Videos
This report has had some controversy for including videos before the creation of the Stars program or
members being added to the program. The list would be smaller if those videos were excluded, but leaving
the old videos could still lead to encouraging unethical behavior such as phishing for data collection.
These videos could have been cleaned up before joining the program, but were not.

### Regarding Robux Giveaway Websites
The Robux giveaway websites that work function by earning money from companies that sell user information
and using some of the earnings to buy gift cards. Giving away gift cards is allowed under the community rules
since they are physical items that are not coupled to Roblox's systems. They are included in the report
since they are advertising data collection websites. Data collection is now allowed under the terms of use,
and is illegal to due for the majority of the target audience (children under the age of 13) in the United States
under the Child Online Privacy Protection Act (COPPA). While the YouTubers can not be prosecuted for advertising
these services since they are not doing the data collection, this is an unethical behavior and should not be
supported by Roblox. Several YouTubers have claimed that Roblox considers the websites to be safe.

One exception for this is that websites will not included if they do not do data collection. Most sites offer watching
videos, but are included since they have offering surveys and downloading apps that require creating accounts
if valid emails. A few websites have pledged to removing surveys, but none have done so, and thus none are excluded
from the report.

## Methodology
The list of videos is generated using data inside a database collected using YouTube's web endpoints. Videos are
added to the database if the title or descriptions contain keywords (such as "giveaway" and "free robux"
ignoring the case) and manually checked before being added to categories (see: Categories). Videos can also
be manually added if they are not caught by the keywords. These will mostly have "Other" as a category.

### Limitations
Videos that hide content in the video (ex: 100 Robux to a random group member each video) is also missed since the
system only checks for the title and description. Since Roblox's Video Stars program is multi-linguistic,
keywords are missing for some languages and others have basic manual checking since the content of the video can
not be understood.

Using keywords for searching has the limitation of not being flexible. There is a bias in the report towards websites
because they do not have any flexibility. For example "roblox.com" can not be changed to "r0blox.com" without it being
an incorrect website. English sentences can be rewritten in many different ways. For example, all of the following
are equivalent, but only 1 would show up for "Robux Giveaway":
* Giving Away 1,000,000 Robux!
* 1,000,000 **Robux Giveaway**
* 1,000,000 Free Robux \[GIVEAWAY!\]
* Need Robux? Giveaway here! 1,000,000 Robux!

While using the keyword "Robux" would show all of these, and was attempted in the first scan, it resulted in tens
of thousands of false positives for saying that Robux is the virtual currency when putting "What is Roblox?" in
the description. "Giveaway" is a keyword used, but does require manually filtering out giftcard giveaways, and is
allowed under the terms of services since it is the same as giving away a physical item.

### Implementation Details
The system has been rewritten several times to improve finding videos and scanning them. Python was chosen for the
wide support in academia and industry, as well as the variety of libraries. The steps for generating the report can
be split into several steps.

#### Build the Cache Database
Due to the the amount of videos, a local cache is required to store the information. Without it, a full scan
of all videos would take 2-3 days and can not be easily re-attempted if it fails. Full scans are needed to find
new videos and for the keywords being changed. The cache building is done in 2 steps.

The first step is to get the ids to populate. This is done by using Selenium (a tool used for automation
testing of websites and other user interfaces) to load the page of the channel's videos and scroll to the
end. Unlike the first iteration that used YouTube's search APIs, this guarantees all public videos by a channel
will get added.

The second step is to add the video data to the populated video ids. Since YouTube dynamically loads data,
Selenium is also used to simulate viewing the video and gather the information from the loaded page. This 
ensures that titles and descriptions are completed compared to using the search API, but is very slow. This process
takes 2-3 days for all videos, but can be easily restarted since video data is saved periodically. If a crash
happens after 60,000 videos, those 60,000 videos are skipped. These can be updated later using several options,
including:
- Update unpopulated videos (described above)
- Update all videos (>200,000 videos)
- Update marked videos (<20,000 videos), which are the ones that are scanned below.

#### Scan The Videos for Keywords
Using the cache database from before, all of the titles and descriptions are checked for a specific set of ~50 keywords.
If at least one matches, it is added to the database to be checked over in the next step. If a video was marked before
and either the title or name changes, they are marked as changed in the database to be re-evaluated, even if the still
match keywords. This is done since a video's description can have a false positive before, and a change could be made
that is still a false positive. Video titles and descriptions can also be changed to have or remove unethical content.

After each scan, a "delta report" is created. These includes the videos that were added or changed as part of a scan.
This is used to determine if there are new videos to look over, or if progress has been made to reducing the amount
of unethical videos.

#### Manually Filter Videos
When videos are added to the database for matching certain keywords, they are manually approved or marked as safe.
This is done because of the connotation of words being fine in some cases but not in others. For example, a "hacker"
in a YouTube video could refer to someone else exploiting, the YouTuber telling people how to exploit, or a player
that is near-perfect enough to appear as an exploiter.

#### Generate The Report
The markdown file is generated from the database. The sections for the intro (this) and the conclusion are stored
as separate files, with the bullet lists being generated. Sorting is handled by name for the channels and by
publish time for the videos.

## Possible Resolutions
The videos in the report have various resolutions. These include:
* Deletion: The videos can be deleted.
* Privation: The videos can be made private.
* Modification of Videos: If the only offending part is the description or title, both can be modified to not include the content.
  *  For videos that contain parts that are on the list, like a 30 second ad, it may be able to be
     trimmed out using YouTube's video editor: https://support.google.com/youtube/answer/9057455?hl=en
* Modification of Rules: The rules of the terms of use can be modified to allow specific behavior such as giveaways.

### Case for Rule Modification - Robux Giveaways
The ban on Robux Giveaways is an arbitrary rule (not backed by a regulation where Roblox operates) and is relatively
easy for Roblox to modify, enforce, or remove. The rule is not enforced outside of extreme scenarios where automated
systems would be able to detect a large amount of random transferring of funds, such as 1,000 transfers of 100 Robux
within an hour. The rule is not enforced outside fo these cases because of the scope required to enforce it. The only
way to be able to enforce this is with automation, similarly to how card network fraud and YouTube's demonetization
systems are automated.

The results of this rule being enforced would be disastrous for developers and group owners. Group owners of social
groups with management roles, such as military groups, or roleplay employees, such as restaurant groups or airline
groups, will pay Robux to people of these roles for maintaining the group. Developers will also pay other developers
or contractors for assets and transfer them outside of Roblox. In both these cases, Roblox has no way of seeing what
is being paid for, and can be interpreted as a Robux giveaway. It is likely that the group owner get banned or even
terminated. If the account is not terminated, the account would probably be ineligible for exchanging Robux for real
money (Developer Exchange, also known as DevEx).

A high profile developer with personal connections to Roblox Developer Relations (DevRel) could get it resolved, but
most developers would need to go through Roblox Customer Support. Roblox Customer Support has the reputation of not
being able to get anything done, meaning a developer could potentially be denied from their only source of income, as
well as anyone receiving payments. This theoretical situation makes Robux more risky as a currency than real money,
which should not be the case given Robux transfers do not require sharing personal information.

## Categories
The videos in the report are categorized based on the content since multiple rules are broken. These include:
* Robux Giveaways: Videos containing or referencing Robux giveaways. This is against Terms of Use 4(A).
  * Giftcard giveaways are excluded from this since they are allowed under Roblox Community Rules 12.
  * If another category exists for the videos (such as “Information Collection” or “Phishing”), the video
    will be part of the other category.
* Information Collection: Videos containing or referencing websites that collect and sell user data and promise
  something in return like Robux or Builders Club. This against Community Rules 2.
* Phishing: Videos containing "hacking fans". These videos work by getting the username and password for an account
  and claiming they are hacking the account. This creates a run-away effect of more users sending usernames and passwords
  to content creators and creating more videos. This gives actual phishers the chance to get accounts and do damage to
  the user. This is against Roblox Community Rules 2, 6, and 10.
* Gambling: Videos containing gambling of Robux or Limited Items. This is against Roblox Community Rules 10.
* Exploiting: Videos containing exploiting, including buying or using. This is against Roblox Community Rules 12.
* Other: Videos containing or referencing other violations. Typically, there will be less than 3 for each category.

Ideally, more categories like “Online Dating” would be included, but is difficult to automatically find based on keywords.
(see Limitations under Methodology).

## Videos
### Unlisted, Private, and Deleted Video Notation
For the list below, *unlisted videos* are italic. They are not removed from the list since unlisted videos can be made
public at any point. Video creators should not be obligated to delete private videos since there may be dependencies on
these videos, such as channel metrics to attract sponsors. Deleted videos will not appear on the list because deleted 
videos can not be un-deleted, which means they can not be referenced in the future. Private videos are also not included
since they can't be discovered or watched by viewers. Private videos may be made public in the future, and will be re-added
to the report if this happens.

### Video Metrics
* Total videos: 253,191 videos
* Total videos found that match keywords: 14,273 videos
  * Total unprocessed videos: 2,728 videos
* Total videos found that are processed and marked: 2,565 videos 
  * Information Collection: 1,596 videos
  * Non-Giftcard Robux Giveaways: 851 videos
  * Other: 92 videos
  * Phishing: 26 videos

### No Videos Found
The following channels had nothing appear with manual searching. Videos may exist, but were not found.
* 3SB Games (Cakemiix and ododahs)
* Alex (CraftedRL)
* Aline Games (AliineGamesYT)
* AlvinBlox (Alvin_Blox)
* Amberry (Amberrry)
* AnielicA (ANIELICA01)
* Anix (iamAnix)
* Ashleyosity (itsAshleyosity)
* Augus (augusvidal)
* Ayzria (Ayzria)
* Bandi (BandiRue)
* Bandites (Bandites)
* Bebe Adri (BebeAdriYT)
* BereghostGames (snapple43, pixiedust423, Bereghost, and valadin1)
* Bia Gamer (BiiiaGamerYT)
* BLAH SQUAD (BLAH_Squad)
* Blox4Fun (SgGamersDad, GustTv, and SimasGamer)
* bobjenz 
* BramP (bramp1996)
* Brianna (zBriannaGamez)
* Buur (BuurmanTenus)
* ByDerank (ByDerank_YT)
* Captain Tate (CaptainTate21)
* Cari - Roblox (Carihyper)
* Carol TV (coolforlovato)
* cazum8 (cazum8)
* Cerso (Cerso93)
* Chocoblox (Chocoblox_93)
* Chrisandthemike (chrisatm)
* Collins Kosuke (Kosukei)
* ComfySunday (ComfySunday)
* Conor3D (Conor3D)
* CPat (StarC0de_CPAT)
* Crainer Roblox 
* Cryptize (Cryptize)
* CrystalSims (CrystalSims)
* cybernova games (THEREALCYBERNOVA)
* DandanPH (DandanPH)
* Danny Jesden (DannyJesdenYT)
* DaPandaGirl (Da_PandaGirl)
* Daylins Funhouse (DaylinsFunhouse and FunHouseDadWasTaken)
* DeeterPlays (DeeterPlays)
* Devovo (Devovorya)
* DoctorBenx (Bensonheimer and ebruulein)
* Dois Marmotas (diinhaaa28 and Luckke23)
* DOLLASTIC PLAYS! (DollasticDreams)
* DraconiteDragon (ItsDraconiteDragon)
* DUDU Betero (BIANOBETER0, DuduBetero, SimoneBetero, and RafaBetero)
* EthanGamer (EthanGamerTV)
* Evanbear1 (Evanbear1)
* faeglow (faeglow)
* Febatista 
* FGTeeV (FGteev and DrizzMcNizz)
* Fraser2TheMax (Fraser2TheMax)
* frenchrxses (french_rxses)
* FUDZ (fudsim)
* Furious Jumper (furi0us_jumper)
* GabriellaGames 
* Gallant Gaming (GallantGaming)
* GianBlox (GianBloxBRz)
* Godenot (godenot)
* GoldenGlare (GOLD3NGLARE)
* GoldenOwl (GoldenOwlYT)
* Gravycatman (GrumpyGravy)
* Hagazo (Hagazo9)
* hannnahlovescows (hannnahlovescows)
* HelloItsVG (HelloItsVinh)
* Hey Sant (heysant2018)
* Honey The Unicorn - Roblox (HoneyTheUnicornYT)
* HulkBR (hulkbr_oficial)
* Hyper - Roblox (DylanTheHyper)
* HyperCookiie (HyperCookiie)
* iBeMaine (ibemaine)
* iGuz (iGuzGames)
* iKotori (iKotori)
* Inemafoo (Inemajohn)
* It's Akeila (itsakeila)
* iTownGamePlay \*Terror&Diversión\* (iTownGamePlayYT)
* Its SugarCoffee (SugarCoffee123)
* ItsFunneh (Funnehcake)
* J-Bug (J_Buggo)
* Janet and Kate (KittyJanet and Kate9071)
* javie12 (javie12)
* Jayingee (jayingee)
* Jeancof ツ (Jeancof)
* Jeenius (RealJeen)
* Jenstine (jenstine and jenstinex)
* JoeyDaPlayer (UseStarCodee_JOEY)
* JonesGotGame (JonesGotGame)
* Jr e Mi (JuniorGuimaraes and micandeloro)
* Julia MineGirl (Crisminegirl and JuliaMinegirl)
* JunRoots (JunioRoots)
* jvnq (jvnqYT)
* Karim Juega (karimjuega)
* Kawaii Kunicorn Gaming (kawaii_kunicorn)
* Kelogish (Kelogish)
* Kepu The Cat (KepuTheCat)
* Kindly Keyin (KindlyKeyin)
* KIRON (SoyKiron)
* KraoESP (KraoESP)
* Krystin Plays (KrystinPlays)
* Lakshart Nia (Laksharth)
* LankyBox (LankyBoxGamesAdam)
* Lawlieet 20 (XLAWLIEETX)
* Leah Ashe (NotLeah)
* Legolaz (LegolazYoutube)
* Lokis (lokis9340)
* LunaBlox (Luna4Dangelis and DeiakZ)
* Lunar Eclipse (Lunar3clispe)
* Magikarp Films (Magikarpfilms)
* Manucraft (ManucraftYT)
* Mary - Frozencrystal 
* mayrushart (mayrushart)
* MeganPlays Roblox (TheMeganPlays)
* MelzinhaMel Games (MelzinhaMelGames, Detona\_Anderson, and Princesa\_Alicinha)
* MIANNN (MIANNNGAMER)
* Michaels Mayhem 
* MicroGuardian (MicroGuardian)
* Milo Mine (BebeMiloAmiwito)
* MooseCraft Roblox (MooseCraftRoblox)
* MrLokazo86 (Lokazo86)
* NapkinNate (NapkinNate)
* Natasha Panda (NatashaPandaaYT)
* Nezi Plays Roblox (NeziPlaysROBLOX)
* NightFoxx (Night_foxx)
* NO\_DATA (NO\_DATA)
* noekje (noekje)
* Oblivious (ObliviousHD)
* PaintingRainbows (RainbowsYT)
* Pankayz (Pankayz)
* Papile (jupapile)
* PeetahBread (PeetahBread)
* PHMittens (Youtube_PHMittensRBX)
* Phoeberry (Phoeberry)
* PinkFate Games (PinkFateYT)
* PinkLeaf (RenLeaf)
* Playonyx Tech (okgamerman)
* PrestonGamez (PrestonPlayz)
* Pretzel Etzel (pretzel_etzel)
* ProSidu (prosiduzao)
* Raconidas (Raconidas)
* RadioJH Games (audreyradio)
* Remainings (Remainings)
* Robotz (R_obotz)
* RoTomek (RoTomek1)
* Ruby Games (ruby_rubeYT)
* Ryguy - Roblox (ryguyrocky)
* SallyGreenGamer 
* SDMittens (SDMittens)
* ShanePlays (SGC_Shane)
* Shaylo (YTshaylo)
* skyleree (skyleree)
* Spagz Blox (Spagzox)
* SrPrismatico (SrPrismatico)
* TanqR (TanqR)
* TapWater (UseCode_Tap)
* Tex HS (TexWillerHS)
* THE KAPOLAR (Kapolar1)
* The Star Squad Gaming (StarSquadMolly)
* TheAtlanticCraft (TheCrazyMaverick)
* Thinknoodles (ImNotThinknoodles)
* ThnxCya (NotThnxCya)
* ThunberGames (ThunberGames)
* TinenQa (TinenQa1)
* TinyTurtle Roblox (BeautifulTinyTurtle)
* TitanHammer Roblox (TitanHammerYT)
* TroyanoNanoReturns (TroyanoNanoReturns)
* TW Dessi Gaming (TW_Dessi)
* TwiistedPandora (TwistedPandora)
* Vitória MineBlox (vitoriamineblox11, anamineblox11, and JackieMineBlox11)
* XdarzethX - Roblox & More! (xdarzethx)
* Yokai (Yokai_YT)
* Z00LD 
* ZaiLetsPlay (YT_ZaiLetsPlay)
* Zerophyx (Zerophyx)
* ZNac (Z_Nac and BlakeBroYoutube)
* ZULY (ItsZulyYT)

### Videos Found
* Ant (Cringley)
  * Other
    * So I MADE A ROBLOX GAME.. It's called DETECTIVE (Roblox)
      * Description mentions giving Robux for liking the game, which is not possible since there is no way to check if someone liked the game. It is a scam, and also an attempt at social engineering.
      * URL: https://www.youtube.com/watch?v=l_wC-1-3J2k
  * Phishing
    * GIVING A FAN 1,000 ROBUX AFTER HACKING HIM!!!
      * URL: https://www.youtube.com/watch?v=p920X7kUXmE
    * HACKING A FANS ROBLOX ACCOUNT!!
      * URL: https://www.youtube.com/watch?v=viMN3GP9p04
    * HACKING MY FRIENDS ROBLOX ACCOUNT WHILE HE WAS SLEEPING!!
      * URL: https://www.youtube.com/watch?v=KiJbY9mkYMc
* Arazhul (Arazhulino)
  * Other
    * VIRUS SIMULATOR?! - Roblox \[Deutsch/HD\]
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=FD6SJcbDIAY
* AshleyTheUnicorn (ashelycoin)
  * Phishing
    * HACKING A FAN AND GIVING THEM FREE ROBUX IN ROBLOX!
      * URL: https://www.youtube.com/watch?v=cpnK6Dnu2M4
  * Non-Giftcard Robux Giveaways
    * How To Get FREE GAMEPASSES In Bloxburg and Tips And Tricks! (Roblox)
      * Video shows giving away Robux using group funds for specific amounts based on paid access rates for games.
      * URL: https://www.youtube.com/watch?v=9skyudqZPkg
    * LIKE THIS VIDEO AND GET FREE ROBUX! How to Get Free Robux!
      * Claims to giveaway 100 Robux per day using group funds.
      * URL: https://www.youtube.com/watch?v=7cfDn34L1Q0
    * JOIN MY ROBLOX GROUP FOR FREE ROBUX!  | Roblox
      * Claims to giveaway 100 Robux per day using group funds.
      * URL: https://www.youtube.com/watch?v=wWQXls9N7Nw
    * WIN FREE ROBUX WHEN I DIE IN THIS ROBLOX GAME!
      * Gives away Robux using group funds as part of a challenge.
      * URL: https://www.youtube.com/watch?v=DLRQLwcL6MI
* Baxtrix (DefinitelyNotBaxtrix)
  * Other
    * SNEEZING On 9999 PEOPLE in Sneezing simulator! // Roblox
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=Sl0h1X-pXfs
* Betroner y Noangy (Betroner and Noangy)
  * Non-Giftcard Robux Giveaways
    * Robux para ti! Mejor que los eventos de Roblox!
      * Video is a Robux giveaway.
      * URL: https://www.youtube.com/watch?v=PqnyTfRPixw
    * TRUCO! Consigue DINERO y XP muy rápido en Loomian Legacy Roblox en Español
      * Contains link to a Roblox giftcard giveaway.
      * URL: https://www.youtube.com/watch?v=0m4FxrqvE-A
* Biel Henrique (BielHenriikOficial)
  * Information Collection
    * DOIS NOVOS ITEMS GRATIS NO ROBLOX CORRA PARA PEGAR O SEU
      * Description references the data collection website earnrobux.online.
      * URL: https://www.youtube.com/watch?v=7MyeJoYmXes
    * COMO FICAR RICO NO ROBLOX
      * Description references the data collection website earnrobux.online.
      * URL: https://www.youtube.com/watch?v=TPOUmRrXrO0
    * ESSE ITEM TE DÁ ROBUX GRÁTIS NO ROBLOX
      * Description references the data collection website earnrobux.online.
      * URL: https://www.youtube.com/watch?v=dPMqEpZWWA0
* Calixo - Roblox Adventures (BuBreezy)
  * Non-Giftcard Robux Giveaways
    * Giving A Noob 50,000 Robux!! (He Went Mad!)
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=YYWu5J6ryPo
* Cylito (Cylito)
  * Non-Giftcard Robux Giveaways
    * Bloxburg: Victorian Roleplay House 62K
      * Description includes a building contest for Robux.
      * URL: https://www.youtube.com/watch?v=cD8ZvszEXf4
* DefildPlays (DefildPlays)
  * Information Collection
    * ALL Adopt me "HACKS" and Secrets In Adopt Me! Roblox Adopt Me
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=zAvtjbmQAyA
    * SECRET Challenge Gives Free MEGA NEON PANDA In Adopt Me! Roblox Adopt Me!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=kDyYCJoT4Ww
    * Adopt me CANCELLED Weekly Updates Forever..!? Roblox Adopt Me
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=sbwV4m9dtDk
    * LIMITED EGGS Are BACK In ADOPT ME!? Safari Egg, Jungle Egg and MORE! Roblox Adopt Me
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=3mXUfabSirU
    * New BUCKS Trading & Doctor Role in Adopt me!? Roblox Adopt Me Ideas
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=qIrTIbJV5vg
    * We Found a SECRET MONKEY PET Treasure Map IN ADOPT ME! Roblox Adopt Me
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=nlFEWE2X9mk
    * I Sold HOTDOGS In Adopt Me For 24 HOURS And THIS HAPPENED! Roblox Adopt Me How To Make MONEY
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=DKlRxvzYcEc
    * TRADING PIRATE ACCESSORIES PACKS ONLY In ADOPT ME! Roblox Adopt Me
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Op9blZplSfE
    * HOW TO GET A FREE PIRATE HOUSE AND PIRATE ACCESSORIES IN ADOPT ME UPDATE! Roblox Adopt me
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=12aa-BU6SiA
    * WE BOUGHT THE NEW PIRATE HOUSE IN ADOPT ME! Roblox Adopt Me
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=67rwsroy4II
    * I GOT A MEGA NEON TURTLE AND NEW MONKEY IN ADOPT ME! \*BEST LEGENDARY PET\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=cfkN0lPIkAo
    * 6 SECRET Ways To Make MILLIONS Of MONEY In ADOPT ME!! \*How To Make MONEY in Adopt Me Roblox\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=BcANtmNa5hc
    * New CIRCUS PETS And MORE In Adopt Me UPDATE?! \*LEGENDARY Monkey pet\* Roblox Adopt Me!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=DH1JVS87FUI
    * Trading UNLIMITED Robux Items For MEGA NEONS In Adopt Me Update!  \*25,000 Robux\* (Roblox)
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=dXTYWtbftQU
    * SPENDING 25,000 ROBUX ON MEGA NEON PETS + ACCESSORIES IN ADOPT ME?! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=3vPSmbwhacE
    * \*NEW\* FREE MEGA NEON PETS IN ADOPT ME UPDATE!! \*MAX UPGRADE\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ni30s-d-CjE
    * This PRESENT Gives A FREE RAINBOW NEON Pet In Adopt! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=u8z7KbAqRjg
    * How To Get A FREE Legendary GOLDEN RAT In Adopt Me!! \*LIMITED PET\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=1SUjtehwUrU
    * Hidden LIMITED \*NEON\* PET ACCESSORIES In Adopt Me UPDATE! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=UbVCnNIaJJQ
    * ADOPT ME SECRET \*RAINBOW\* NEON PETS UPDATE!? \*LEAKED\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=CVoWm1JpKDY
    * Adopt Me \*HIDDEN\* Free PET ACCESSORY Challenge!! \*CRAZY\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=yB11IzxUdjQ
    * ADOPT ME FREE New PET ACCESSORIES Event! NEW Adopt Me Update (Roblox)
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ZRDgSOzpxzg
    * HOW TO GET \*FREE\* PET ACCESSORIES IN ADOPT ME UPDATE!! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ybQu5D51v2E
    * NEW Secret \*FREE\* GOLDEN BUNNY PET Codes In SPEED CHAMPIONS! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=_fiZk5lszvI
    * Bubble Gum Simulator BUT Every Trade Is A Random Legendary!? \*FREE Secret Pet?!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=vn4LMIT4sf4
    * ALL 49 EGGHUNT 2020 LOCATIONS!! FabergEgg, Eggveloper EGG, X Y Z EGG! \*How To Get\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=XHHUovaz0YY
    * FREE STAR CREATOR EGG + GETTING ALL 2020 EGGHUNT EGGS IN ROBLOX EGGHUNT 2020!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=pjJkuYwTVsI
    * ALL 6 SECRET OWNER BANHAMMER CODES IN SIZZLING SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=qVUGahY260I
    * I Used 40X LUCK To Get ALL SHINY MUSHROOM PETS In ROBLOX BUBBLE GUM SIMULATOR! \*BROKEN\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ydx2psrYJyo
    * BLIND TRADING Fans DARK ELEMENT PACK Pets In Roblox Ninja Legends!! \*MOST OP\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Ej_FSiGIfu0
    * UNLIMITED MAXED DARK ELEMENT PETS IN NINJA LEGENDS \*Giveaway!\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=0VaLEwqgc6Y
    * 🍄 MYTHICAL MUSHROOM Pet CODES In Bubble Gum Simulator!! \*AMAZING PETS\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=rpDXE-WOGGY
    * FREE DARK ELEMENT PET PACK CODES IN NINJA LEGENDS! \*SECRET CRYSTAL\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Rs7ZqPXA0ro
    * How To Get UNLIMITED "SECRET PETS" In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ogI88kRe0y0
    * 14 GLITCHED Free OWNER Pet Codes In Blade Throwing Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=IzwNhktxQJM
    * Opening Mythical Egg For All SECRET PETS For 24 Hours In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=IolPVTW2qds
    * Why Roblox Ninja Legends Is NOT updating...
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=7pXid9SD7Q4
    * I GOT EVERY NEW MYTHIC PET IN BUBBLE GUM SIMULATOR!! \*MAX REWARDS PETS\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=qkYIUDoSQ9k
    * MYTHICAL DEMONCORE PET Update Codes In BUBBLE GUM SIMULATOR!! \*GIANT UPDATE!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=1P0dUx8zjm0
    * SECRET MYTHICAL PET Codes In Roblox SODA SIMULATOR! \*INSTANT MAX RANK!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=tTJxxrkU18w
    * ⚔️ Defeating NEW LAVA GOLEM For \*BROKEN\* ANCIENT PETS In BLADE THROWING SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Q0ccUh86noQ
    * 🙊 This NOOB Has $1,000,000,000,000 RAINBOWS In Roblox Bubble Gum Simulator! \*RICHEST NOOB!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=eB6JCYgFtKA
    * ❄️ I Got NEW Secret SHINY FROST SENTINEL Pet In Roblox Bubble Gum Simulator! \*Best Pet\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=nSRnEdj7bgw
    * BOUGHT The $250.000.000.000 TOXIC DEMON And Then THIS HAPPENED In BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=uHJxnnSMmIs
    * I HATCHED THE SECRET CLOVER EGG PET IN BUBBLE GUM SIMULATOR! \*1 IN 8 MILLION\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=BilC3fYHyZ8
    * NEW SEASON 7 FREE PREMIUM BUBBLE PASS CODES IN BUBBLE GUM SIMULATOR! \*Giveaway!\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=dmRKtbcStI8
    * 👉 INFINITE FREE ZXLEGENDS PETS AND PACK IN NINJA LEGENDS! \*GIVEAWAY\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=DX8gN0UH6yU
    * ALL 34 ZX LEGEND PET PACK CODES IN NINJA LEGENDS! \*SUPER OP\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=rXh-9PSirWo
    * 14 SECRET FREE ANGELIC PET CODES IN BLADE THROWING SIMULATOR! \*SUPER OP\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=erWIgMRxqvk
    * 💰 INFINITE FREE ZX LEGENDS PETS IN NINJA LEGENDS! \*FREE MOST OP PETS\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=BgxRPv62lRk
    * FREE 0.001% ZX LEGEND PET PACK CODES IN NINJA LEGENDS! \*SUPER OP\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=yqiVEeFPEi0
    * 😱 NEW FREE GALACTIC SHOCK PET CODES IN BUBBLE GUM SIMULATOR UPDATE! \*SUPER OP PETS\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=905KTXjZVy8
    * ✨FREE HYBRID MOON PET UPDATE CODES IN SPEED CHAMPIONS! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=UCzkG6F1Vdc
    * 🍀 TRADING A LUCKY VIEWER A MAX ENCHANT LUCKY TOPHAT IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=2ARjjibuE9o
    * 😲 I TRADED FREE CHAOS TITAN PACKS PETS IN NINJA LEGENDS! \*Giveaway\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=maLi-UoRg0A
    * ALL 10 MAXIMUM GEM PET CODES IN BLADE THROWING SIMULATOR! \*2T GEMS FREE!\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=EUQWqO7F_vQ
    * 💰 23 SECRET SHIPWRECK GEMS UPDATE CODES IN FISHING SIMULATOR! \*FREE GEMS\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=vRA00Lip8o8
    * 😱 NEW SECRET CHAOS LEGENDS PET CODES IN NINJA LEGENDS UPDATE! \*BEST SWORDS AND RANK!\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=PMxt7D4fFno
    * 😲 ALL LEGENDARY PORTAL PET CODES IN BUBBLE GUM SIMULATOR UPDATE! \*OP INSTANTLY\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=AaRgEMtBodc
    * 💰 6 SECRET HYBRID LAVA PET CODES IN SPEED CHAMPIONS! \*BEST PETS\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=x3k_ErTp91Q
    * 😲 TRADING FREE SECRET INDEX PETS IN BUBBLE GUM SIMULATOR! \*Giveaway\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=lvwoQ0bZXFg
    * UNLIMITED FREE CHAOS TITAN PETS IN NINJA LEGENDS!! \*Roblox\* (Giveaway)
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=j_N7QNxMhLo
    * 2 FREE MYTHICAL SCYTHES AND SECRETS IN ROBLOX MINING SIMULATOR!
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=5aQW3cJdNT8
    * NEW ROBLOX BEACH SIMULATOR RAINBOW CRAB! ! + SHADOW SCYTHE GIVEAWAY WINNER!
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=8c8B8qNVHHg
    * ROBLOX TOYS SERIES 3 BLIND BOXES OPENING AND FREE TOYS CODES!
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=7CxkZXK3qDE
    * WHY POKEMON BRICK BRONZE IS DELETED ON ROBLOX BY NINTENDO! \*Sad\*
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=zYk8r-A9NXc
    * (Code) NEW SWAMP REALM, LEGENDARY QUESTS AND ULTIMATE RAYGUN TOOL In Roblox Mining Simulator!
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=EWwvD903Y4g
    * \*Omg\* NEW RORIA LEAGUE AND LEGENDARY POKEMON UPDATE SOON IN Pokemon Brick Bronze!?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=k3L-0FA8rjk
    * SOLVING THE FINAL DOMINUS VENARI CLUE!! WHO WILL GET IT!? - Roblox Ready Player One
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Y9YYYbSRRR0
    * GETTING THE GOLDEN DOMINUS CUBES! FINAL CLUE FOR THE DOMINUS! - Roblox Ready Player One
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=83a8rScHf88
    * \*NEW\* EASTER EVENT, FREE LEGENDARY EGG, NEW EASTER ORES AND MORE In Roblox Mining Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XWf-oebYlZA
    * HOW TO GET THE 6TH FESTIVAL DOMINUS CUBE! \#6/8 - Roblox Ready Player One
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=udV9_v_DQf8
    * WE FOUND THE GOLDEN DOMINUS ENTRANCE?! HELP US CRACK THE CODE!- Roblox Ready Player One
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=vdmPFTkBDnQ
    * HOW TO GET THE GOLDEN DOMINUS GATE CUBES FOR THE FINAL CLUE!? - ROBLOX READY PLAYER ONE EVENT!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=xwshl1QO3IU
    * GETTING EVERY EGG IN ROBLOX EGG HUNT 2018 - 100.000 SUBSCRIBER SPECIAL!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=h66wWSwc7_c
    * \*Omg\* THIS BEAR OUTFIT GIVES SO MUCH HONEY! INSANE BEAR BEE In Roblox Bee Swarm Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=GAW7EA3Sugo
    * WE SOLVED THE CLUE LETS GO! GETTING THE GOLDEN DOMINUS! | Roblox Ready Player One Event
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=QRmYh_KkYB8
    * HOW TO GET THE TEST BADGE AND THE SECRET PORTAL TO THE GOLDEN DOMINUS CLUE! Roblox Ready Player One
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=boT80L28OSw
    * WE FOUND THE DOMINUS GAME! GETTING THE GOLDEN DOMINUS! | Roblox Copper, Jade, and Crystal Key
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=gggdvItTZZY
    * WE FOUND A NEW CLUE!!! GETTING THE GOLDEN DOMINUS! | Roblox Copper, Jade, and Crystal Key
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Aw4_-n74cAE
    * THE GOLDEN DOMINUS IS 100% IN THIS GAME! (New Clues!) - Roblox Ready Player One Event!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=QNUB1h5qjN0
    * (Omg) HOW TO GET FREE THOUSANDS OF HONEY AND FREE ROYAL JELLY IN Roblox Bee Swarm Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ufNwXNRTUUc
    * (Codes) ALL UP TO DATE TWITTER MONEY CODES In Roblox Mining Simulator! \*SO MANY FREE ITEMS!?\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=E9vn1PCJXfs
    * I GOT THIS FOR ONLY 100 ROBUX?! - Roblox Legendary Hat Opening In Roblox Mining Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=kPegxinrC2g
    * NEW LEGENDARY HATS UPDATE, PRIVATE MINING AREA AND NEW ORES In Roblox Mining Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=BkADs-KTLsA
    * \[NEW CLUE!!\] SEARCHING THE GOLDEN DOMINUS! (HOW TO GET EVERY KEY) - Roblox Ready Player One
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qFR7OzR4T4Y
    * \[NEW CLUE!!\] SEARCHING THE GOLDEN DOMINUS! (HOW TO GET EVERY KEY) - Roblox Ready Player One
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=0-jnWBP2SJs
    * HOW TO GET THE CRYSTAL KEY AND CROWN! \*Full Tutorial+Solver!\*  (Roblox Ready Player One Event)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XgfKR4RDf5g
    * GETTING \#1 REBIRTHS IN ROBLOX TREASURE HUNT SIMULATOR!? \*1 Rebirth Every 10 Seconds\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=skyyhW2xCOQ
    * \*Omg\* HOW TO REBIRTH EVERY 10 SECONDS IN ROBLOX TREASURE HUNT SIMULATOR! \*Elite Pack Gamepass OP\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=kmCzF_l93nw
    * HOW TO GET THE JADE KEY LIVE! COME JOIN TO GET IT! - Roblox Ready Player One Event!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=q0-bxxTv9pA
    * NEW SANDSTONE REALM, DARK HOLE TOOL AND EMERALD TREASURE In Roblox Treasure Hunt Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=8GzslpLjUoo
    * (Code) NEW ITEM SHOP UPDATE IN ROBLOX FORTNITE! - Island Royale \*Come Join!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=9vEcKNip34s
    * HOW TO GET THE JADE KEY IN ROBLOX! \*Puzzle Solver Included!\* - Ready Player One Event!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jibjt0Q4VPc
    * \*Easy\* HOW TO GET THE COPPER KEY IN LESS THEN 10 MINUTES! - Roblox Ready Player One Event!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=KnIcz3p8yNw
    * I'M GETTING THE COPPER KEY! \*JOIN TO KNOW HOW!\* - Roblox Ready Player One
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=I0TBUI72lWQ
    * HOW TO GET THE COPPER KEY QUICK AND EASY! \*Full Tutorial\* - Roblox Ready Player One Event!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=wXTZzYi4F9w
    * BIG NEW CLUE FOR GETTING THE COPPER KEY! - Ready Player One - ROBLOX EVENT
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=n-xq4KGmh4k
    * NEW CLUE FOR GETTING THE BRONZE KEY! - Ready Player One - ROBLOX EVENT \*Trying Every Clue\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=scxpvZB6DQM
    * \*New Clue!\* THE BRONZE KEY IS LOCATED IN THIS GAME?! - (Roblox Ready Player One EVENT)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=tT7tBLXH6ZA
    * SEARCHING THE GOLDEN DOMINUS! - Ready Player One ROBLOX EVENT
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ry5V1A1lB-k
    * THE 1 CHEST CHALLENGE! - Roblox Fortnite - Island Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=6KJF1VJkW1M
    * (Codes) NEW SPACE REALM AND VIDEO EXCLUSIVE SKIN CODE In Roblox Mining Simulator! \*Insane Money!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=l3OnB5A2DeM
    * (Code) OWNER GAVE ME ALL WORKING SECRET CODES IN ROBLOX MINING SIMULATOR! \*So MANY ITEMS!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Q1iSwPwsjG8
    * MOST OP SECRET SPOT IN ROBLOX FORTNITE ISLAND ROYALE! \*You Have To Try This!!\* /W Rexex
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=5YkHehUrQEw
    * BUYING THE GOLDEN NUKE AND EXPLODING THE WHOLE PRIVATE ISLAND In Roblox Treasure Hunt Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=aHf1K-gDi_Q
    * NO SCOPE ONLY CHALLENGE In Roblox Fortnite \*Insane!\*- Roblox Island Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XLZ9o5AXVG8
    * (Codes) NEW REALMS, LEGENDARY PETS, MYTHICAL SCYTHE IN Roblox Mining Simulator \*THIS IS INSANE\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=5iPRaEubjBQ
    * \*NEW\* SNIPER ONLY CHALLENGE VICTORY in Roblox Fortnite: Battle Royale! - Island Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=a-tS2VgnTLs
    * Buying PEGASUS Pet and Making $166.000 EACH SECOND In Treasure Hunt Simulator! \*BEST MONEY MAKER\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=V9AY9LIq1gw
    * 300M LONGSHOTS VICTORY ROYALE - Roblox Fortnite - Island Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ytslSlDFp6s
    * WINNING MY 2 FIRST ROBLOX FORTNITE GAMES EVER! \*Alpha Access Giveaway\* - Roblox Island Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=zzExupK1mPc
    * NEW EARTHERN REALM AND LEGENDARY PETS! \*Make MILLIONS EACH MINUTE!?\* Roblox Treasure Hunt Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=RGY6qz4g0x0
    * \*Omg\* BIG INSANE LEGENDARY CHEST OPENING IN ROBLOX BOOGA BOOGA!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=gMHyFZK45kI
    * HOW TO RIDE MAMMOTHS, SHARKS AND EVERY ANIMAL IN Roblox Booga Booga! \*SUPER Insane\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Qx4mQgA5Yas
    * GETTING READY FOR ROBLOX FORTNITE BATTLE ROYALE! \*Come Play\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cIN20sVqkGY
    * SEARCHING THE BIGGEST GOLDMINE IN Roblox Booga Booga! \*Epic\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=0jYWSozXQhg
    * BEST WAY TO FIND GOLD AND THIS ITEM COSTS 140.000 ROBUX In Roblox Booga Booga!?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=_W9FDXnLqhk
    * FINDING THE SECRET SPIRIT LAIR IN Roblox Booga Booga! \*Insane Exp\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=3kzlhKGQ6YA
    * (Omg) HOW OP IS THE LEGENDARY ZUES STAFF IN ROBLOX MINING SIMULATOR!? \*4800$ EACH SECOND\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=iaNHGkBIYm8
    * GLITCHING TO 8000+ BLOCKS, LEGENDARY TREASURE CHESTS IN ROBLOX MINING SIMULATOR! \*Making Millions!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Qg78fHRUork
    * (Code) OWNER GAVE ME ALL THESE SECRET MONEY CODES FOR ROBLOX MINING SIMULATOR! (Free $10.000's )
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Y92OHq3djQs
    * HOW OP IS THE LEGENDARY NUKE IN ROBLOX MINING SIMULATOR! \*4000 Coins Every Second!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=iA6W5QgwNH0
    * (Code) ALL 2018 CODES AND FREE INSANE BACKPACK IN Roblox Mining Simulator! \*1000's Of $\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=nRNzZNeZ_J8
    * (New) ALL 6 UP TO DATE MONEY CODES IN TREASURE HUNT SIMULATOR! \*Easy Money\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=SM4tR3wXhBQ
    * EXPLORING TOMBS FULL OF TREASURE IN ROBLOX EXPLORER SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=fQSCbMGrDBo
    * (WUT?) THE WEIRDEST SIMULATOR IN ROBLOX!? \*My Viewers Suggested This!\* - Roblox Lawn Mower Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qFQ7YbCKTY8
    * (Code) ALL NEW POSSIBLE TOOL SKINS AND PETS UPDATE IN TREASURE HUNT SIMULATOR!? \*What Do You Want?\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=l6SXdBPDGeY
    * NUKE DELETES ALL SAND ON PRIVATE ISLANDS IN ROBLOX TREASURE HUNT SIMULATOR! \*Insane Sand!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=WHPn7811Oos
    * ROBLOX FORTNITE BATTLE ROYALE RELEASE INFO! \*All You Need To Know\* - Roblox Fortnite
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=nrKIkF6sGWg
    * (Code) NEW TOOLS SKINS, PETS, NEW ISLAND In Roblox Treasure Hunt Simulator? \*Decide The Update!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=mArPrp_4u6w
    * REMOVING ALL THE SAND IN TREASURE HUNT SIMULATOR \*1000's Treasure Chests!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=7nf6BDhNUAE
    * LUVDISK VALENTINE EVENT IN POKEMON BRICK BRONZE!? \*Come Spread The Love!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=dzlCJd29AJo
    * HOW TO REBIRTH EVERY 17 MINUTES \*Alone\* In Roblox Treasure Hunt Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=beXRGEriT2c
    * (2 Codes) NEW PETS, NEW ISLAND AND NEW CHAINSAW UPDATE IN ROBLOX EXPLORER SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=K9PkzWS1-lc
    * THE BEST TOOLS TO USE IN TREASURE HUNT SIMULATOR?! \*WORST TO BEST\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=YhmT0yZ2wnY
    * HOW OP IS THE LEGENDARY ☢ NUKE ☢ !?  \*BLOWING UP THE WHOLE MAP!?\* - Roblox Treasure Hunt Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=sFXjDY3U7RI
    * (Code) NEW MYTHICAL REALM, NUKE, AND LEGENDARY CHESTS IN ROBLOX TREASURE HUNT SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ZxAtBX-SHgI
    * HOW OP IS THE LEGENDARY DRILL AND SCOOPS!? \*6500$+ EACH SECOND!!\* - Roblox treasure hunt simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=zE9pOAmeewE
    * (3 Codes) NEW INSANE MONEY GOLD MINE AND NEW BOSSES \*SUPER FUN\* IN ROBLOX EXPLORER SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=BU1z4wu5aSU
    * DIGGING TO THE BOTTOM OF TREASURE HUNT SIMULATOR! \*WHATS THAT?\* - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=EHhDUsSKaXk
    * \*OMG\* THE ULTIMATE ROCKET FUEL CAR RACE IN ROBLOX JAILBREAK!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XPoWiDKdsEI
    * (How To) AUTOMATICALLY SHOVEL SAND AND GOING TO THE BOTTOM OF ROBLOX TREASURE HUNT SIMULATOR!?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qxUOr1zyUl8
    * HOW TO GET OMNITRIX MASTER CONTROL IN BEN 10 ARRIVAL OF ALIENS! \*Instant Alien Transformation!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cJX8QjfIhe4
    * HOW OP IS THE LEGENDARY INFINITE BACKPACK!? \*$100.000.000 IN 1 GO!\* - Roblox Treasure Hunt Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=fXhBAYW7GJg
    * HOW TO GET FREE ROCKET FUEL EVERY DAY IN ROBLOX JAILBREAK- New Jailbreak Update
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=bl9J8MJcqUo
    * NEW BINOCULARS, MCLAREN SUPER SPEED AND BOX GLITCH UPDATE! - Roblox Jailbreak
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=JFFSz2gX7yw
    * (Code) ALL UP TO DATE 2018 CODES IN ROBLOX TREASURE HUNT SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=QXeVLJBn4eo
    * HOW OP IS THE LEGENDARY GOLDEN SPOON!? \*4000$ EACH SECOND!!\* - Roblox treasure hunt simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=34FOf7O0EAU
    * (Code) NEW UNDERWORLD, GOLDEN SPOON AND PRIVATE TREASURE ISLANDS UPDATE IN TREASURE HUNT SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=nFWmIJ3d9Iw
    * \*INSANE\* NEW BEN 10 ULTIMATE ALIEN ABILITIES AND ALIEN UPDATES! (Ben 10 Arrival Of Aliens) / Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=9TjcHKkTqPI
    * (Code) \*RAGE\* BACON HAIR STEALS MY SPECIAL LEGENDARY TREASURE CHEST IN TREASURE HUNT SIMULATOR!!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=xJenMD8aSnM
    * (Code) GETTING THE INSANE LEGENDARY TOOL IN ROBLOX TREASURE HUNTER SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jbgIIXTw2Ns
    * (Code) NEW BOSS ADDED / ICE CAVE REVAMP / NEW BOSS PETS In Roblox Snow Shoveling Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=H9AO6pa_Y08
    * \[CODES\] \*NEW\* GETTING INSANE LEGENDARY CHESTS IN ROBLOX TREASURE HUNT SIMULATOR?!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cct9itil1SI
    * ALL SECRET 8TH GYM UPDATE EASTER EGGS YOU MISSED In Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=634Ude_w7cQ
    * HOW TO GET SANDYGAST / PALOSSAND In Pokemon Brick Bronze! \*Beach Ghost Mystery\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cRTz8qHLusE
    * SEARCHING FOR KYOGRE CLUES IN POKEMON BRICK BRONZE! - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=beLMXYEP_hA
    * (Live) PLAYING THE NEW COUNTER BLOX REMASTERED?! - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jON4XGrHG0w
    * \*OMG\* INSANE Ben 10 VS VILGAX (Ben 10 Arrival Of Aliens) - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=RNL3FtuwsJY
    * (New) HOW TO TRY ON ANY CATALOG ITEM ON ROBLOX! \*Try Before You Buy\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XCx2dMtP9WY
    * LEGENDARY GIVEAWAY AND GETTING SHINY GOODRA! - POKEMON BRICK BRONZE - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=rx-7CRLL8u0
    * NEW SNOWBALL FIGHTS UPDATE IN Roblox Snow Shoveling Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=TXaHCrwFyZM
    * HOW TO GET BOUND (MINI) HOOPA In Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=wWJKbrx4Yqw
    * (NEW CODES) ALL \*WORKING\* 2018 CODES in CASH GRAB SIMULATOR - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ICoNfOMsLeY
    * GETTING SHINY CELEBI AND HOOPA AND GENESECT! - POKEMON BRICK BRONZE - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=OfbkOem0hTY
    * New INFINITE ICE BACKPACK, ICE SPEAR And 2 NEW PETS In Roblox Snow Shoveling Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=bgqZBitqXrM
    * (2 Codes) NEW SNOWBALL FIGHTS AND ANIMATIONS UPDATE! Roblox Snow Shoveling Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=C6IL5Jn8C6A
    * Getting The NEW GOLDEN HAMMER And SILVER FIRST In Roblox Mining Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cpHnMgG5awI
    * HOW TO GET GENESECT IN POKEMON BRICK BRONZE! \*8th Gym Update\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=1MXHMctgUgs
    * HOW TO GET HOOPA IN POKEMON BRICK BRONZE! \*8th Gym Update\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Q2pJ8jpExFk
    * (Update) NEW INSANE ALIEN VILGAX ADDED! \*Super Strong!\* (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=UDnuc_MmFX0
    * 8TH GYM RELEASED! SOLVING ALL SECRETS LIVE! - POKEMON BRICK BRONZE - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=WAzog27nswA
    * HOW TO GET CELEBI AND THE GS BALL IN POKEMON BRICK BRONZE! \*8th Gym Update\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Z-S1uK0Y80s
    * MY CHARACTER IS IN THE GAME!? \*Stealing From Myself\* - Roblox CashGrab Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=_n6bLZ32w7o
    * PREPARING FOR THE 8TH GYM UPDATE SOON! - POKEMON BRICK BRONZE - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=adk1WXaRsAI
    * (Code) NEW ICE KING SPEAR And FASTEST WAY FOR ICE KING BACKPACK In Roblox Snow Shoveling Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=5EcOtndYRa0
    * HOW TO SELL GAMES ON ROBLOX! \*Insane Money\* - Roblox Cashgrab Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=MYjIDOt1hnw
    * \*Professional editor!\* GETTING The OP MEGATON SWORD In Roblox Mining Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=gO2e7lVhxzs
    * ROBLOX Bans FREE ROBUX And Youtuber Pokediger1! - \#RobloxWatch - BIG Roblox Terms Of Service Change!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=lp9-EveEdfQ
    * (Update) NEW TOOL!  IT'S BETTER THAN LIGHTSABER - Roblox Snow Shoveling Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=54GJlLofx0w
    * (NEW CODES) ALL \*WORKING\* 2018 CODES in SNOW SHOVELING SIMULATOR - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qmBSq6tz9QU
    * (Code) HOW TO GET A FREE LIGHTSABER AND ICE TOOLS In Snow Shoveling Simulator - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=lq5Kl95HPLY
    * PREPARING FOR THE 8TH STEEL GYM AND MAGEARNA UPDATE! - POKEMON BRICK BRONZE - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=n3mlQk9aq5I
    * Roblox PERM BANNED LandonRB! \#RobloxWatch MEME NOT ALLOWED And Tacticles Reaches 100K!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=-8f3_HGkun8
    * (Code) FASTEST WAY HOW TO GET ICE IN THE NEW SNOW SHOVELING SIMULATOR UPDATE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=iAFtFREfKEw
    * (NEW) WHAT IS INSIDE THE ICE GOLDMINE CAVE IN SNOW SHOVELING SIMULATOR UPDATE! - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=6Ktcb9U1gQU
    * (NEW) ICE MOUNTAIN EXPANSION AND LIGHTSABER?! - Snow Shoveling Simulator - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=PjedKC2tDcQ
    * (NEW UPDATE) HOW TO SPAWN THE ICE KING IN SNOW SHOVELING SIMULATOR - ROBLOX
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=f2iQhZBxzuY
    * PREPARING FOR THE NEW 8TH GYM AND NEW POKEMON UPDATE! - Pokemon Brick Bronze
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ZBV6ZupkSKM
    * THE 8TH GYM TYPE AND MAGEARNA UPDATE LEAKED!? - POKEMON BRICK BRONZE - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=aiPGBMCQ7Q8
    * NEW 8TH GYM AND NEW POKEMON UPDATE LEAKS! - Pokemon Brick Bronze
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=50E0rqWBXwU
    * \#DUCKSQUAD PERMANENTLY BANNED \#RobloxAlert FREE ROBUX IS FORBIDDEN?, NEW ROBLOX DEVELOPER LOGO!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=BVxJSyVqe1s
    * \*Epic\* GLITCHED RANDOM ALIEN OMNITRIX BATTLE! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=5ys3SLYPJrs
    * INVISIBLE SNOW COP TROLLING IN JAILBREAK! \*THEY CAN'T SEE ME\* - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=l3lGoPtHotA
    * SHINY LEGENDARY GIVEAWAY AND PREPARING FOR 8TH GYM UPDATE IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=QJDXvgL36bo
    * \*INSANE\* GETTING THE MOST OP VEHICLE IN SNOW SHOVELING SIMULATOR! - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=gI9MgqldJpQ
    * \*NEW\* OMNI ENCHANCED CANNONBOLT VS OMNI ENHANCED GREY MATTER! - Roblox Ben 10 Fighting Game
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=pq_afTSN2co
    * ENDLESS HORDES OF ZOMBIES INFILTRATE ROBLOX! \*HELP US\* (Roblox Zombie Attack)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=BKdle4-fB1o
    * \*UPDATE\* NEW OMNITRIX, MOBILE VERSION AND ALIEN MENU IN BEN 10 ARRIVAL OF ALIENS!?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=tMZraqLbu0I
    * \*NEW UPDATE\* BLACK HOLE PET AND FREE DIAMOND SNOW PETS ON SNOW SHOVELING SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=LQV3xk_2BKo
    * PREPARING FOR THE NEW 8TH GYM AND HOOPA UPDATE! - Roblox Pokemon Brick Bronze
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qB8OcBQIXDw
    * BEN 10 OMNI ENCHANCED HEATBLAST VS OMNI ENCHANCED XLR8 (Ben 10 Fighting Game)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=E-qmudUSHDo
    * PREPARING FOR THE 8TH GYM AND HOOPA UPDATE! - Roblox Pokemon Brick Bronze
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=v6ugg4fPtSg
    * HOW TO INFINITE AFK SNOW SHOVELING GLITCH! - Roblox Snow Shoveling Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=mxlhDpjZim0
    * HOW TO GET INTO THE BEN 10 NULL VOID! - Roblox Ben 10 Fighting Game
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=-YlMnXlj1bY
    * HOOPA, 8TH GYM AND CRESENT ISLAND UPDATE INFO AND MORE! - Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=NKOfNAOqufk
    * \*NEW\* ALL OMNI ENCHANCED ALIENS UPDATE IN BEN 10 - Roblox Ben 10 Fighting Game
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Gf0pD8qMbFo
    * \*INSANE\* WAY BIG EVOLUTION Of Aliens REMATCH! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=BrmU4Em3emM
    * I HAVE BECOME ROBLOX IN ROBLOX!? \*SUPER CRAZY\* (Would You Rather..?)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=AnR6tXEljOk
    * My 2017 RECAP + PLANS FOR 2018! \#PinkArmy / DefildPlays
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=9Q4bC4y0eMk
    * BACON HAIR FINDS SECRET CHRISTMAS LEGENDARY In Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=HXMzG0M1fKw
    * BEN 10 GETS A SHINY CHRISTMAS SCEPTILE!? - Roblox Pokemon Brick Bronze
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=YQOJf3c_gOo
    * Ben 10 DARK OMNITRIX vs LIGHT OMNITRIX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=N99zQHTXaj8
    * \*OMG\* How to FLYING TRAIN GLITCH in ROBLOX JAILBREAK!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=mHiGkaqWlmQ
    * \*OMG\* We Get A NEW OMNITRIX? BIGGEST MYSTERY In Ben 10 Arrival Of Aliens!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=un73qyVMCJ0
    * \*NEW\* Ben 10 ICE OMNITRIX vs EVIL OMNITRIX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=EPYSupaRxAA
    * \*FREE\* NEW BEN 10 ALIEN SANTA GIVES FREE ROBUX PRESENTS!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=9-SQCoknrLY
    * \*New Alien\* HOW TO BE SANTA On Ben 10 Arrival Of Aliens On ROBLOX! \*Super Strong\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=xE2wKQPuNGY
    * \*OMG\* BEN 10 SAVES CHRISTMAS IN ROBLOX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=76bVXBvVZ0s
    * \*WINTER UPDATE\* TRAIN ROBBING / NEW BANK HEIST / 1 MILLION BIKE AND MORE! (Roblox Jailbreak)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=eAmz6Vf0K8U
    * \*OMG\* BEN 10 ALIEN ROBS THE NEW TRAIN IN ROBLOX JAILBREAK!? (Ben 10 Jailbreak)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=xVI0kTAnmCg
    * GETTING MEGA CHRISTMAS SCEPTILE!? \*WINTER EVENT UPDATE\* (Pokemon Brick Bronze)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=-9i6PHeh968
    * \*NEW\* Ben 10 GHOST OMNITRIX vs NORMAL OMNITRIX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=yPwufoAKBVM
    * \*OMG\* THESE Aliens DEFEAT Ben 10!? INSANE ULTRA Aliens! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=RI6akNRZ6u8
    * \*NEW\* HOW TO GET THE CHRISTMAS SCEPTILE C MEGA STONE In Pokemon Brick Bronze! \*Christmas event\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=fwHJ-QHidKQ
    * \*EPIC\* NEW BEN 10 ULTIMATRIX UPDATE + NEW PLAYER MODELS?! (Ben 10 Arrival Of Aliens
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=RMKpZv7QUP4
    * \*OMG\* NEW Jailbreak TRAIN ROBBERY And TRAIN UPDATE!? (ROBLOX Jailbreak)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=et_5K8A5dqI
    * \*NEW\* Ben 10 FIRE OMNITRIX vs WATER OMNITRIX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=8cXuXj5RWCw
    * \*OMG\* 1 YOUTUBER VS 5 OMEGA ALIENS IN BEN 10?! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=6kTPfLmJY48
    * BIRTHDAY ULTRA BEN 10 VS YOU! (Roblox Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Hz2P3wx7JAc
    * BIRTHDAY LEGENDARY HUNTING STREAM! - Roblox Pokemon Brick Bronze
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qyUoFFWp3sg
    * \*NEW\* BEN 10 ALIEN ABILITY CARDS! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=kPSr1VZY2rs
    * \*Omg\* INSANE Ben 10 WAY BIG Vs HUMUNGOUSAUR! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ZL2QpKlb4jA
    * \*IMPORTANT\* We Need To Talk.... (Click This Video If You Are Subscribed)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jrHr2CnPwqw
    * \*Epic\* New LAVA OMNITRIX Random Aliens GLITCH!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=lN1Cl4LOWrE
    * \*Insane\* STRONGEST Alien In BEN 10? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Es19-QZ5QHg
    * REACTING to "ROBLOX MUSIC VIDEOS: THE MOVIE 2" by Buur
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=-O5QBOz26Fk
    * \*FREE\* HOW TO GET THIS INSANE FREE WINTER PRESENT From ROBLOX!? (Dominus, Robux or More?!)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=mc0Q2Rq5GPE
    * \*OMG\* Kevin 11.000 VS ULTIMATE ALIENS IN Ben 10! (Ben 10 Roblox)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ym91p_gLGrk
    * \*New\* All BEN 10 ULTIMATE Aliens VS ULTIMATE Aliens! (Roblox Ben 10) - Charity \#11
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=BRcro4nSmyM
    * \*VLOG\* CATCHING NEW GENERATION 3 MONS in Pokémon Go!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=IRPJf9vhmwk
    * BEN 10 VS MAD BEN 10 IN ROBLOX! \*REMATCH\* (Ben 10 Arrival Of Aliens)  - Ben 10 For Charity \#10
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=nET6bRDRKuI
    * \*EPIC\* New ALIENS RATH VS CHROMASTONE In Ben 10!? (Roblox Ben 10)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=HQfFgXJgFgA
    * \*MOST INSANE COMBACK\* 26 VS 10 VICTORY ROYALE?! - Fortnite 50 VS 50
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ReEzJvBWieE
    * \*Omg\* INSANE ULTRA Alien TRANSFORMATIONS!? (Ben 10 Arrival Of Aliens) - Ben 10 For Charity \#9
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=thsIDHvqqq8
    * SO MANY VICTORY ROYALES!? \*MY WORLD RECORD\* 50 VS 50 UPDATE! - Fortnite Battle Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=5Dj6hijO9d0
    * \*EPIC\* NEW GOLD OMNITRIX GIVES THESE RANDOM ALIENS!? (Ben 10 Arrival Of Aliens) - Roblox Charity \#8
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=m_q4NkOdLQ8
    * \*OMG\* NEW Alien CHROMASTONE And Ben 10 GAME? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=NPHXyhgDbDk
    * \*OMG\* BEN 10 Vs FANS! \[SO MANY EPIC BATTLES\] (Ben 10 Arrival Of Aliens) 31 Days Of Ben 10 \#7
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=iJVs0Z38PPQ
    * \[NEW SERIES\] OMG THAT SCARED ME SO MUCH! | Underrated Roblox Games \#1 \[Roblox Alone\]
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=-SRX9H6KclE
    * \*INSANE\* The MOST OP ALIENS In Ben 10 Challenge! (Ben 10 Arrival Of Aliens) - Ben 10 For Month \#6
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=eEgcTz9yoPQ
    * NEW \*SUPER CAR\* And SNIPER In JAILBREAK WINTER UPDATE!! - Roblox Jailbreak
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ON9GgVSfgCs
    * \*Omg\* Insane GOOP ULTRA EVOLUTION! (Ben 10 Arrival Of Aliens) - Ben 10 For Charity \#5
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jMdG1TCpdqk
    * \*EPIC\* INSANE NEW Random Alien ULTRA OMNITRIX!? (Ben 10 Arrival Of Aliens) Ben 10 For Charity \#4
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=eQJKoIU2z9w
    * \*EPIC\* INSANE ULTRA CANNONBOLT BATTLE! (Ben 10 Arrival Of Aliens) - 31 Days Of Ben 10 For Charity \#3
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=3hHMVEj5NjY
    * GETTING EPIC AURA POKEMON IN POKEMON BRICK BRONZE! \*ROBUX GIVEAWAY EVERY AURA\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=bBaXUr8Q4CY
    * \*Update\* NEW Insane AURA POKEMON And The SAFARI ZONE In Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=GHf9rchZZfw
    * \*OMG\* If You Find THESE ALIENS You GET 20.000 ROBUX!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=R46I9bIO9bI
    * \*EPIC\* ULTRA ALIEN BATTLE VS BEN 10! (Ben 10 Arrival Of Aliens) - 31 Days Of Ben 10 For Charity \#2
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=xQhGWlrvWg8
    * \*EPIC\* Getting A BLIND VICTORY!? MMX Released In ROBLOX! (Roblox MMX)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=3nfDXWeWSW8
    * \*NEW\* INSANE Ben 10 ULTRA BATTLES! (Ben 10 Arrival Of Aliens) - 31 Days Of Ben 10 For Charity \#1
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=oAOz4Yxcj1Q
    * \*UPDATE\* New Alien RATH And FUTURE UPDATES! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=zQpLikkS1qc
    * \*Insane\* EPIC ALIEN TRANSFORM EVOLUTIONS! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=_vwUojM-Tow
    * \*WHAT\* If YOU GUESS THEM, Get $100,000 ROBUX! (Roblox)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=koaJTSANT4E
    * \*EPIC\* NEW RAINBOW Random Alien OMNITRIX! (Ben 10 Arrival Of Aliens) - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=PkolNB3udXY
    * \*UPDATE\* NEW BEN 10 WINTER MAP AND ALIENS! (Ben 10 Arrival Of Aliens) - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=TA9DMdwUd1E
    * MY CRAZY GRANDMA TRIED TO EAT ME IN ROBLOX! - Roblox Roleplay
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=79s4NTNZk2k
    * \*Ultimate\* EVOLUTION of ALIENS TRANSFORMATION! \*Challenge\* (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=HiiDmJH_XY0
    * \*OMG\* THe MOST Epic BEN 10 VS Random EVOLUTION Aliens! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=HFeEHj7XTyY
    * \*NEW\* Insane MULTIPLE Mode! 2 ALIENS At Once! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=5hPg8ogmMCQ
    * BEN 10 GETS A POKEMON! - Roblox Ben 10 Arrival Of Aliens Roleplay
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=_5Y8-zM2Q_E
    * INSANE LEGENDARIES! Randomizer NUZLOCKE! (Pokemon Brick Bronze) \#5
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=d68UiUa-KIk
    * \*INSANE\* WAY BIG Vs GREY MATTER!! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=QR4wZQCgWnw
    * \*Insane!\* Big Chill EVOLUTION Of Aliens! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=sLlDz299TgE
    * \*Epic\* New OMNITRIX Random Alien GLITCH!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=3D6eJU9VPfw
    * WE BOUGHT OUR NEW HOUSE!! BUT THEN THIS HAPPENED! (Roblox Roleplay)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=IdR3wAHULRc
    * \*Epic\* Insane WAY BIG vs XLR8 Alien Battle! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=YCXaq04Yp0M
    * ARCEUS MADNESS! Randomizer NUZLOCKE Challenge! (Pokemon Brick Bronze) \#4
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=k4hjpwtU05I
    * \*OMG\* These EPIC Aliens! VS Ben 10?! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Iabc1OBa8Js
    * \*New\* Insane Superhero JUSTICE LEAGUE in ROBLOX! \*Get Superpowers!\* (Injustice Online Adventure)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=fyt0Szi87Jc
    * WE GOT ARCEUS! Randomizer NUZLOCKE Challenge! (Pokemon Brick Bronze) \#3
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=VaA2s-TR8Q4
    * \*Omg\* NEW Epic Ben 10 CLONING machine VS ALBEDO! (Ben 10 Movie Special)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=hRjDmD68l9M
    * \*Omg\* NEW Random OMNITRIX Transforms In These ALIENS!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jlLr5NMfEwM
    * \*INSANE\* NEW Aliens In Ben 10! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=PeueHloQDKY
    * MORE UNIQUE POKEMON!? Randomizer NUZLOCKE Challenge! (Pokemon Brick Bronze) \#2
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=pTYUMaWMdhY
    * \*INSANE\* CANNONBOLT EVOLUTION Of Aliens! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=nBX4bfNb4UI
    * LEGENDARY STARTER!? Randomizer NUZLOCKE Challenge! (Pokemon Brick Bronze) \#1
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=1cyI_em_a7A
    * \*Insane\* New Aliens RATH And ATOMIX And Future UPDATES!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ZobGnN2d3nw
    * \*Epic\* How to get ANY LEGENDARY as your STARTER Pokemon In Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=md5GEd-u5_I
    * \*Omg\* How to be INVISIBLE / Go THROUGH Walls EVERY ALIEN!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=mrAI2IB3Q0k
    * \*Epic\* NEW Random Alien OMNITRIX!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=AXWGbROJk6o
    * \*Epic\* GWEN 10 Vs ALBEDO In ROBLOX (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=KOdSd5s6QHk
    * BEN 10 Arrival Of Aliens THE MOVIE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=PZqnQ5ITWQs
    * \*This Is Insane\* I get THOUSANDS Of Robux... FOR FREE!? (Roblox Robux Simulator)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=f0qNmfYvy4c
    * \*Epic\* 10 INSANE SECRETS and Places In BEN 10! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=4iJfP-LTOIg
    * The 8th GYM, Ultra Beast NECROZMA And SAFARI Zone In Pokemon Brick Bronze!?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XL9JPEBEe9E
    * \*Insane\* FASTEST Alien In BEN 10? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=c9S9n5iEf8E
    * \*Insane\* HUMUNGOUSAUR EVOLUTION Of Aliens BATTLE! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=EVApOF07dPY
    * \*Epic\* INSANE Skateboard / Skatepark UPDATE With BEN 10 In Roblox! (Robloxian Highschool)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=wDZyUubouvo
    * \*Insane Update\* New BIG ALIEN REVAMPS! ( Xlr8 And More ) (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=RrG9dldaM8k
    * \*New UPDATE\* How To Get KEVIN 11 In Roblox! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=EneXA9h37uY
    * NEW ALIENS!! Battle VS Ben 10! (Kevin 11, Way Big) (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ojH2UlBvFzc
    * \*Insane\* New FREE RARE DAGGER In Swordburst 2! (Ruby Edge Dagger)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=r5GD37uzCG4
    * Bacon Hair Finds HIDDEN SECRET LEGENDARY Teleport In Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=z-t5EJ321YY
    * \*NEW\* ALIENS And Abilities Revamp UPDATE! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=2XlDuRz0WZs
    * \*Epic\* NEW Ben 10 REVAMP ALIEN Battles! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=7D2_hGWN2UI
    * \*INSANE\* Sword Art Online IN ROBLOX!? (SwordBurst 2)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=o2BqKUXfDxY
    * \*INSANE\* WAY BIG EVOLUTION Of Aliens BATTLE! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ZE8qkXcg3wA
    * \*OMG\* PLAY THE NEW BEN 10 REVAMP!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Cw9sWNIQ4U0
    * \*Live\* Getting SHINY Lugia in Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=OYJf_JkbDyE
    * NEW ALIENS!! Battle Vs BEN 10 (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=PDiah_BpUvc
    * \*NEW\* ALIENS and future updates! \[Atomix, Rath, Chromastone\] (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=yVB0zoHXOUo
    * \*Secret\* How to get LUGIA in Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=A51X7qCS1Gs
    * \*New\* Alien Big Chill And Epic New Player Attacks! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=CBidHFd5_5I
    * BEN 10 vs ZOMBIE BEN 10 in ROBLOX 👻 (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=tw4y14SH9J8
    * Halloween Evolution of Aliens! 👻 (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=zvVLen9fRXw
    * NEW ALIENS and ABILITY UPDATES! \[Kevin 11, HeatBlast, Anubis\] (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Ohr2_viIScs
    * GIANT TITAN TROLLING in TITAN SIMULATOR! (Roblox Titan Simulator)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ufS-xFYuR2k
    * \*NEW\* ALIENS and EPIC NEW ABILITIES \[Big Chill, Anubis, Feedback\] (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Q2JFZM2J-jA
    * BEN 10 uses ADMIN COMMANDS to TROLL IN ROBLOX!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=3BvQzIG2NcY
    * BACON HAIR FINDS HIDDEN LEGENDARY GATE IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jvCb7-RJPB0
    * NEW EVOLUTION OF ALIENS MODE! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ZrKYuLfxwpI
    * WARZONE BOSSES, EPIC CRATES AND MORE! | MINECRAFT SKYBLOCK X \#4 | (ArcadianMc)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=fUWqXkyx23o
    * BEN 10 VS BEN 23 IN ROBLOX!! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=3Tw9UXQKnCc
    * SECRET NEVER SEEN HALLOWEEN ALIEN ADDED!! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=frqqDImcYFk
    * EPIC NEW ALIENS! BATTLE VS BEN 10 (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ALjuldoBk3s
    * INSANE WARZONE BOSS BATTLE | MINECRAFT SKYBLOCK X \#3 | (ArcadianMc)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=2CK7NXPrtGE
    * NEW HALLOWEEN EVENT ZOMBIE MODE IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=7aN-5jvtICk
    * SHINY HALLOWEEN LEGENDARY HUNTING IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=TFk9nINnlt8
    * THIS ITEM MAKES ME PRO!? | MINECRAFT SKYBLOCK X \#2 | (ArcadianMc)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=_elXtYSFdkU
    * BACON GIRL HAS THIS SPOOKEY LEGENDARY IN POKEMON BRICK BRONZE?!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=OSrwNS8OLj0
    * THE INSANE SKY JOURNEY BEGINS! | MINECRAFT SKYBLOCK X \#1 | (ArcadianMc)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=i7M-WHA3eqc
    * BEN 10 VS BAD BEN 10 IN ROBLOX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=17ZsVY0oLSs
    * SPECIAL HALLOWEEN POKEMON BATTLE IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=74-KoY7N9w0
    * NEW ALIENS!! BATTLE VS BEN 10 (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=1_75TFG0T7U
    * HOW TO GET A FREE SHINY MIMIKYU IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=LJDnkRQr5Ok
    * BACON HAIR FINDS LEGENDARY PORTAL IN POKEMON BRICK BRONZE \*Halloween Event\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Fy0X4dIWH4Q
    * THE FLASH VS ZOOM IN ROBLOX (Roblox The Flash)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=uFBDKnf7hCA
    * GETTING SHINY HALLOWEEN EVENT POKEMON IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=WcDy77qWfqM
    * WE GOT A SHINY MIMIKYU IN POKEMON BRICK BRONZE!? \*Halloween Event\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jA9MczFlsA4
    * HOW TO GET MIMIKYU IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=2YuFF7sFp8I
    * HOW TO GET MARSHADOW IN POKEMON BRICK BRONZE! \*Halloween Event\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=7vkK6heAf1w
    * THE MOST EPIC SCAVENGER HUNT IN ROBLOX!? \*Must Watch\* / Find The Domos \#1
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qCh2LjxCBfI
    * BACON HAIR FINDS HIDDEN LEGENDARY ENTRANCE IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=OWmsIWb_9v4
    * KEN 10 VS BEN 10.000 IN ROBLOX (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jGI9gqZ9EyI
    * HOW TO GET A GLIDER IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=o3fEEYBLWt4
    * MIMIKYU? NEW HALLOWEEN EVENT IN POKEMON BRICK BRONZE! \[Theory\]
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=TeWUk_O_aUI
    * BACON HAIR GOT A GROUDON IN POKEMON BRICK BRONZE!? \[INSANE\] \*Not ClickBait\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=pU4wHGLiG7I
    * BEN 10 AND GWEN 10 VS CHARMCASTER IN ROBLOX \[INSANE\] (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=HibBUczgG4o
    * GET YOUR OWN AIRPLANE IN POKEMON BRICK BRONZE!? \*Omg\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=NRbPWR-3F_w
    * BACON HAIR GIVES THESE FREE LEGENDARY EGGS IN POKEMON BRICK BRONZE! \*Trolling\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=z1klRQ8noDA
    * \*INSANE\* PENNYWISE IT CLOWN EATS BEN 10 IN ROBLOX! - Ben 10 Arrival of Aliens
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=zZXA2Xr0NE0
    * THE LAST ROBLOX GUEST HAS THIS IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=z578Db6HWjs
    * BACON HAIR HAS THESE SHINY LEGENDARIES IN POKEMON BRICK BRONZE?!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=7HNUNxqgL2g
    * THIS ITEM COSTS OVER 1 MILLION DOLLARS IN JAILBREAK!? - MASSIVE UPDATE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ePW0W6Zufkk
    * BEN 10 VS NEGA BEN 10 IN ROBLOX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qOmRO4u6s-M
    * LEGENDARY KNIFE CASES OPENING IN ROBLOX PHANTOM FORCES
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=bPxhgik49Hw
    * POKEMON PARTY IS BACK?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=wee811lRl5s
    * GWEN 10 MAGIC SPELLBOOK IN ROBLOX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=JIF09YwNSaU
    * INSANE KILLSTREAKS - ROBLOX MOVIE - Stream Highlights \#1
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=gmgDzLWxZuQ
    * BACON HAIR BATTLES THIS INSANE TRAINER IN POKEMON BRICK BRONZE!?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=iZ7axNgI4eM
    * BACON HAIR FINDS PORTAL TO THESE LEGENDARIES IN POKEMON BRICK BRONZE!!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Gr5SJMj27f4
    * NEW FURNITURE UPDATE IN POKEMON BRICK BRONZE!! \*KITCHEN , SWIMMING POOLS AND MORE\* / Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=T8h0p0SwyVc
    * NEW LEGENDARY 2 MILLION DOLLAR APARTMENT IN POKEMON BRICK BRONZE!?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cizRK96NvIs
    * HE GOT THIS FOR FREE?! BACON HAIR HAS THESE LEGENDARIES IN POKEMON BRICK BRONZE! / Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Rjcor6n2kYE
    * THE BEST ROBLOX GAME TO PLAY WITH FRIENDS!! \*Must Watch\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cgfC1YW3Xxc
    * THE 1.5 MILLION DOLLAR APARTMENT IN POKEMON BRICK BRONZE!!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=bLdo02Ggt38
    * BACON HAIR FINDS HIDDEN TOP SECRET ESCAPE IN JAILBREAK!? \[Roblox\]
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=vSshBVgLrng
    * How To Get THOUSANDS Of FREE ROBUX In ROBLOX EVERY DAY!! \*No Roblox Hack\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XC4QIzInssQ
    * THIS HAPPENS IF YOU JOIN TEAM ECLIPSE IN POKEMON BRICK BRONZE! \*DONT DO IT\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cmByYpJ9UsI
    * BACON HAIR FINDS THIS LEGENDARY IN POKEMON BRICK BRONZE!!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=KTNKdtuqWsg
    * HOW TO MAKE 500.000$ EVERY HOUR IN POKEMON BRICK BRONZE!!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=UP-Q00zCrvw
    * 👉 FREE MAXED REWARD EVENT PETS IN BUBBLE GUM SIMULATOR! \*SUPER OP\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=8Ae1_HILUaY
  * Non-Giftcard Robux Giveaways
    * MAX UNLOCKED VALENTINE EVENT PET CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=7nYXOVHOZPo
    * SECRET 20X ANCIENT ULTRA BEAST PET CODES IN NINJA LEGENDS! \*FREE AUTO SWING\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=E6_-S-i23hY
    * SECRET FREE REBIRTH PET TOKEN CODES IN LAWN MOWING SIMULATOR! \*INSANE PETS!\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Du7rTAGeM_w
    * SECRET GODLIKE FREE PET CODES IN VIKING SIMULATOR! \*MAX SWORD AND ISLAND\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zrTsKM4wsQE
    * SECRET INFINITE ZEN ISLAND PET CODES IN CHAMPION SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=mH7cIaN9EqU
    * NOOB GETS SECRET ULTRA BEAST PRO CODES AND BREAKS NINJA LEGENDS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=5K-y_WQR9Eg
    * SECRET UNLIMITED MAX LEVEL ULTRA BEAST PETS IN NINJA LEGENDS!! \*Roblox\* (Giveaway)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=wC_z4mYTvGk
    * SECRET MAX LEVEL MYTHICAL PET CODES IN VIKING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=QgvWKDCHnL8
    * SECRET MAX ELEMENT ULTRA BEAST PET CODES BREAK NINJA LEGENDS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=1da-iNAih9Y
    * ALL 26 SECRET MYTHIC CUB BEE CODES IN BEE SWARM SIMULATOR! \*SUPER OP\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=VwF-yzDfvPk
    * NEW MAX DRAGON LEGEND SWORD CODES IN NINJA LEGENDS!! \*MAX SWORDS INSTANTLY\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=m1YOlnIF_xU
    * SECRET 500 FREE MAX LEVEL Z MASTER PETS NINJA LEGENDS!! \*Roblox\* (Giveaway)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zYiRWgej9N8
    * 25 SECRET ELEMENT Z-MASTER PET CODES IN NINJA LEGENDS! \*MAX PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=vZErczSQfuY
    * NOOB UNLOCKS ALL SECRET ELEMENTS CODES IN NINJA LEGENDS!? \*INSTANTLY PRO\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=DEYqjGpufVE
    * ALL 16 SECRET CRYSTAL PET CODES IN BOSS FIGHTING SIMULATOR!! \*INSTANT OP\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=1_ot9L3MIr4
    * FREE MAX PREMIUM BUBBLE PASS 5 CODES IN BUBBLE GUM SIMULATOR! \*CIRCUS PASS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=0VXRuD6bVF8
    * MAX ELEMENT MASTER ALTAR CODES IN NINJA LEGENDS!! \*INSTANT MAX RANK\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=XjujG-94zu8
    * UNLIMITED MAX STAR EVOLUTION PET CODES IN LAWN MOWING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=YpsymQy1Oas
    * SECRET UNLIMITED MAX LEVEL Z MASTER PETS NINJA LEGENDS!! \*Roblox\* (Giveaway)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=CDG2TgBwCNw
    * INFINITE RAINBOW PETS, MAX RANK AND MONEY IN PET SIMULATOR 2!!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Nxzgb43vi4s
    * NEW INFINITE Z MASTER DARKSTAR PET CODES IN NINJA LEGENDS! \*MAX RANK INSTANTLY\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=kRLKwLgpsBQ
    * I GOT THE 30,000,000,000 TICKETS DRAGON PLUSHY IN BUBBLE GUM SIMULATOR! \*ALL SECRET PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=C8zmNKPmYQ8
    * Noob With Full Team of Z-LEGENDS Pets! x4.68B Boost! MAX RANK INSTANTLY! - Roblox Ninja Legends
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=5bxbyByPHiY
    * SECRET CIRCUS EVENT PET UPDATE CODES IN BUBBLE GUM SIMULATOR! \*INSANE PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=6s7rHCRJ_IM
    * SECRET INFINITE FREE Z-MASTER PET CODES BREAK NINJA LEGENDS! Roblox \*INFINITE MONEY\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=fTF6WckLJlg
    * INFINITE FLOWER CURRENCY 50X PET CODES IN LAWN MOWING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4CLdShxdM4c
    * FREE 3X SECRET ETERNAL HORIZON PACK CODES IN NINJA LEGENDS! \*MUST USE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=5Dwh9iPU6YE
    * SECRET YOUTUBER PET CODES IN HEROES OF SPEED SIMULATOR! Roblox \*MAX SPEED\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Iuzp9VDxeBs
    * FREE UNLIMITED MAX LEVEL X GENESIS PETS  NINJA LEGENDS!! \*Roblox\* (Giveaway)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4B7wjsySi_g
    * FREE MOUNT VIP PET CODES IN LAWN MOWING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=VopEOTr8OV4
    * SECRET INFINITE PETS GAMEPASS CODES IN CHAMPION SIMULATOR! \*FREE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=UNNAdczOn9M
    * 3 SECRET VOID MOON PET CODES IN SABER SIMULATOR!! Roblox \*BEST PET\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=U3yKr15x-wE
    * I Opened The \*GIANT COIN CHEST\* And It \*BROKE\* Pet Simulator 2!! \*INFINITE COINS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Jid6dDWIGao
    * I Found A SECRET QUEST In Ninja Legends For FREE X-GENESIS PETS!? Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=vnRPC9CuBsY
    * SECRET UNLIMITED X GENESIS PETS DUPE NINJA LEGENDS!! \*Roblox\* (Giveaway)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ifg7MdfLL5o
    * 10 HIDDEN \*HACKS\* IN NINJA LEGENDS YOU NEED TO KNOW!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=C2p-Bg8Luhw
    * I Bought A $12 MILLION DOLLAR GAMEPASS In This Roblox Simulator!? \*999,999,999 ROBUX\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ei0c0DvTiqY
    * I GOT UNLIMITED FREE X-GENESIS PETS IN NINJA LEGENDS!? \*Roblox\* (Giveaway)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=TV062MN4rrA
    * SECRET INFINITE FREE X-GENESIS PET CODES IN NINJA LEGENDS! Roblox \*SUPER BROKEN\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=l9WASkeAqco
    * SPAWNING FREE UNLIMITED ELEMENTAL PETS IN NINJA LEGENDS! Roblox \*GIVEAWAY!!\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=aiqtgIXtfnQ
    * WE FOUND 10 SECRET DOUBLE MONEY CODES IN MAGNET SIMULATOR! \*SUPER OP\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=TJ__YD2On5w
    * I Became The \*TOP RANK\* PLAYER In OP NINJA SIMULATOR!! \*SUPER OP\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=EKrMl48WS0E
    * ALL 77 NOOB TO PRO 2020 PET CODES IN BUBBLE GUM SIMULATOR! \*SUPER BROKEN\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=aBBmo_xIpmA
    * NEW BEST RUMBLE QUEST FROST LEGENDARY UPDATE CODES!! \*MUST USE!\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zLtfjqRC6F4
    * UNLIMITED ELEMENTAL PETS ALTAR GIVEAWAY IN NINJA LEGENDS! Roblox \*SUPER BROKEN\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=7Vcc0hFkp78
    * ALL 20 SECRET 2020 PET CODES IN NINJA LEGENDS!! \*FREE MAX PETS!\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=lZH1-ysD1OE
    * FREE 10.000 ROBUX BEST PET CODES IN CHAMPION SIMULATOR!! \*FREE HYBRID GODLY!\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ufAZnKio_qQ
    * ALL 25 SECRET MYTHIC BEE PACK CODES IN BEE SWARM SIMULATOR! \*MUST SEE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=iaiQZSpF2KI
    * Secret NINJA LEGENDS PET CLONING ALTAR 100X CODES! \*FREE ELEMENTAL PETS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=uMxhcFi70WU
    * SEASON 4 BUBBLE PASS CODES, GIFT TREE AND 3 NEW EGGS IN BUBBLE GUM SIMULATOR!! \*MASSIVE UPDATE!\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Pf3kXziUTDI
    * ALL SECRET RUMBLE QUEST GAMEPASS COIN CODES! \*FREE DUAL WIELD\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=i64hWvC5des
    * Getting 9 \*Best\* GOLDEN DESERT PETS And SECRET AREA In PET SIMULATOR 2! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=kjnUIa74rPc
    * ALL CHAMPION SIMULATOR HIDDEN EVENT CANDY CODES! Roblox \*ALL LOCATIONS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4BQ9JUuXimA
    * SECRET OWNER SABER SIMULATOR \*FREE CANDY CANE\* CODES! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=bU0vpxior1E
    * SECRET 10.000 ROBUX MAX PET CODES IN SNOWMAN SIMULATOR! \*UNLOCKED EVERYTHING\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=8x8wkNWRnXA
    * SECRET Champion Simulator FREE HYBRID CHARM Codes!! \*FREE PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=sM8Nn-rQveM
    * FREE SECRET NINJA LEGENDS ELEMENTAL PETS + CODES!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=xa6PG11vz2g
    * TOP 10 BEST ROBLOX GAMES You FORGOT Existed  In The Last Decade!!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=MGchmRmm1OE
    * SECRET PET RANCH SIMULATOR 2 YOUTUBER PET CODES! \*OMG\* (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=RLopE2iaLZA
    * SECRET CHAMPION SIMULATOR FREE HYBRID BOSS PET CODES! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=slfAXfqOIWk
    * FREE CHRISTMAS NINJA LEGENDS AWAKENED PET CODES!?! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=pfth-tN1Xw8
    * ALL 31 SECRET SABER SIMULATOR XMAS PET UPDATE CODES!! \*BROKEN PET!\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zeKrukr-ZhM
    * SECRET CHRISTMAS UNBOXING SIMULATOR UPDATE COIN CODES! \*SUPER OP\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=t8UW0cotfyU
    * ALL PET RANCH SIMULATOR 2 SECRET FREE PREMIUM PET CODES! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=iJfrPfMgw98
    * SECRET CHAMPION SIMULATOR ADMIN PET CODES!?! \*MUST USE!!\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=5sLS_2N8qrY
    * ALL 17 SECRET LEGEND \*CRYSTAL\* PET CODES IN NINJA LEGENDS!? Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=aAhfzEsC0g8
    * Making EVERY LEGENDARY Pet GOLD In PET SIMULATOR 2! \*RAREST PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=MfW3tSZoaWI
    * FREE LEGEND STARSTRIKE PET CODES AND MAX RANK IN NINJA LEGENDS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=6KpRRFYAnOc
    * I GOT THE BEST SABER, AURA AND WINTER PETS IN SABER SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=nxMUQRehROY
    * I UNLOCKED \*NEW\* MAX GOLDEN PETS EQUIPPPED In Pet Simulator 2!! \*NEW SECRET AREA\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=9Rf664ekLDw
    * A HACKER GAVE ME THIS?! FREE INFINITE PETS CODES IN PRESENT SIMULATOR! Roblox \*NOT GOOD\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=2rqMdqGuQxE
    * GOLDEN PETS, FREE VIP DROPS AND ALL SECRET ROOMS IN PET SIMULATOR 2 UPDATE!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=lu-iKwzd8Gg
    * SECRET MAX RANK, FREE DUAL LEGEND PET CODES IN NINJA LEGENDS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=v2HkZEw1Diw
    * I Unlocked EVERY UPGRADE And MAX CASH In PET SIMULATOR 2! \*HOVERBOARD AND MORE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=k5yKGQaf7V4
    * PET SIMULATOR 2 RELEASE! UNLOCKING EVERYTHING + GIVEAWAYS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=UeJv9NY8qQg
    * FREE FOREVER VIP, SECRET CAVES AND MAX LEVEL IN PET SIMULATOR 2! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=f4GELA5StDE
    * ALL SECRET INFINITY PET UPDATE CODES IN NINJA LEGENDS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=9kxAogEaB0A
    * I Got The BEST 0.03% PET, BEST SABER AND CLASS IN SABER SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=fYAi17vnvTc
    * ALL SECRET OWNER GEM COIN CODES IN WIZARD SIMULATOR! \*MUST USE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zyuXbWJT5K0
    * FREE DOMINUS ROBUX PET CODES UPDATE IN CHAMPION SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=trhIkJvruto
    * I Became The RANK 1 PLAYER In NINJA LEGENDS And THIS Happened! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=JgoSa4Qh_qo
    * ALL 11 SECRET LEGEND PET CHI CODES IN NINJA LEGENDS!! \*MUST USE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=qm7cvODuXGs
    * I Unlocked The MAX "TERMINATOR" RANK And SECRET PETS In SABER SIMULATOR!! \*FULL TEAM\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=f5uLwr2xBdg
    * I Unlocked ANCIENT BATTLE MASTER AND FREE MAX LEGEND PET IN NINJA LEGENDS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=0_yx4iMXpHA
    * I Unlocked The BEST AURA, SABER AND PET IN SABER SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=JsVP2BCffSs
    * FREE MASTER LEGEND PET PACK CODES IN NINJA LEGENDS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=flpazxjWkAM
    * FREE MAX LEVEL IMMORTAL PETS GIVEAWAY IN NINJA LEGENDS! Roblox \*BIGGEST GIVEAWAY\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=RDI11xrDJww
    * 6 SECRET FREE MAX RANK GEM CODES IN CHAMPION SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=gxqgFc9Tm58
    * 3 SECRET MAX YEN CODES IN ANIME FIGHTING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zWlWpyeEesA
    * SECRET 150K TURKEY PET OPENING CODES IN SABER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ztXlRL_Duzk
    * TOP 10 PLAYER AND FREE 100M CHI CODES IN NINJA LEGENDS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=UOC6Hl7a3hA
    * FREE IMMORTAL MINI LEGEND AND MAX RANK IN NINJA LEGENDS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=7-Z6RCO1Zyk
    * ALL SECRET FREE THUNDER PET CODES IN NINJA LEGENDS UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=DXQUOYmnDyQ
    * SECRET OWNER TURKEY PET CODES IN SABER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=l3WmiGs8MmQ
    * FREE PREMIUM BUBBLE PASS CODES IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=eDieGjT6ujI
    * ROBLOX MEMES That Are BETTER Then Free Robux!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=iuQnFLiYdwA
    * 17 FREE REAPER PET UPDATE CODES IN REAPER SIMULATOR! \*FREE SHARDS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=TVRO8JqZPGQ
    * FREE MAX LEVEL IMMORTAL PET CODES IN NINJA LEGENDS! \*FREE PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=wi5X92hlnWs
    * SECRET RAREST SHINY DEATHSPEAKER CODES In SABER SIMULATOR! \*WORLDS FIRST\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=3FGgjEkB3lY
    * ALL SECRET FREE IMMORTAL PET CODES IN NINJA LEGENDS! \*MUST USE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=sZLWLidXZK0
    * SECRET RAINBOW GOLDEN PET EGG CODES IN SABER SIMULATOR! \*INSANE PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=K4KV-lWqcDE
    * 3 SECRET IMMORTAL PET UPDATE CODES IN NINJA LEGENDS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=gjjn3J5OE6k
    * SECRET ORACLE AURA UPDATE CODES IN SABER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=u6Yy2Bf4Dd8
    * 18 HIDDEN SOURCE SHARD CODES IN REAPER SIMULATOR!? \*SUPER BROKEN\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=bFEAb8EDjmQ
    * ALL SECRET YEN POWER CODES IN ANIME FIGHTING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=aQ87RKgBK-4
    * TEAM OF "500" MOST OP GLITCHED PETS IN NINJA LEGENDS!? \*DONT TRY THIS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=D6O3Lz47C1A
    * I SPEND 5 BILLION CROWNS ON THIS PET IN SABER SIMULATOR.. \*RAREST PET\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=9Yj-mxtLlWY
    * I GOT A TEAM OF "24" GLITCHED PETS IN NINJA LEGENDS!? \*SUPER BROKEN\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ZzFrLhbto0M
    * 3 OWNER OMEGA INFERNAL PET UPDATE CODES IN MAGNET SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=eaymXHqpQA4
    * 3 SECRET RAINBOW DOMINUS PET CODES IN SABER SIMULATOR! \*WORLDS FIRST\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=XZPMV6FwY6Q
    * SECRET ETERNAL PETS UPDATE CODES IN NINJA LEGENDS SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=k4WR62x9NQI
    * ALL 6 SECRET RAINBOW PET CODES IN CANDY COLLECTING SIMULATOR! \*12 MILLION MULTIPLIER\*Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=yuKk9sahjHk
    * Roblox NOOB vs BULLY vs HERO Story In Saber Simulator! \*Sad\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=KZyJGJbznP0
    * 11 SECRET OWNER SHARD CODES IN REAPER SIMULATOR! \*INSANE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=NayGw4oEzj8
    * I Spent 750M CHI On Pets In NINJA LEGENDS And THIS Is What I Got.. \*MAX POWER\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=q4KphbwFar8
    * HIDDEN ISLAND OMEGA PET CODES IN NINJA LEGENDS SIMULATOR UPDATE! \*MUST SEE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=J_Dwt1Emkc8
    * ALL SECRET EVOLVED ULTRA PET CODES IN NINJA LEGENDS SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=axXCLiYJVvQ
    * FULL RAINBOW PET TEAM UPDATE CODES IN SABER SIMULATOR!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=sEmtnUTHDGM
    * SECRET OMEGA INFERNO PET CODES IN MAGNET SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4duQ-1qN2sg
    * SECRET OVERSEER PET GAMEPASS CODES IN SABER SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=kC-BKTC04Uw
    * ALL 6 HEAVEN PET UPDATE CODES IN HALLOWEEN SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Twtyzr2sIcw
    * HOW TO GET FREE OMEGA HALLOWEEN PETS IN MAGNET SIMULATOR! \*BIG GIVEAWAY\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=CG0xhGYRAAo
    * THE OWNER GAVE ME SECRET BOOST CODES IN REAPER SIMULATOR! \*INSANE CASH\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=qHWg2XwTYQ4
    * 2 SECRET LEGENDARY OWNER CODES IN SABER SIMULATOR! \*INSANE CROWNS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=pvj_pm7_CR8
    * ALL HALLOWEEN CANDY OMEGA PET CODES IN MAGNET SIMULATOR!! \*MUST GET!\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=DU3eM35OfkU
    * SECRET ANGEL PET CROWN CODES IN SABER SIMULATOR! \*MUST SEE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4XXoOi8aUfI
    * How To Get A FREE Shadow Dragon In ADOPT ME!! \*Free Candy\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=J_tKnX5bBSs
    * 56 SECRET HALLOWEEN BOOST CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=3-llkEN5MYE
    * 4 SECRET SHINY DOMINUS PET CODES IN HALLOWEEN SIMULATOR!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Q0p_pxyM79U
    * SECRET YOUTUBER CODES AND FREE LEGENDARY PET IN SABER SIMULATOR!? Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=2YlUcojabFo
    * SECRET EYE PET UPDATE CODES IN MAGNET SIMULATOR! \*LIMITED CODES\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ZpCbSMS3A-U
    * I OPENED 2000 HALLOWEN EGGS IN SABER SIMULATOR!! \*INSANE SHINY PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=U_UwwMLbsjE
    * ALL HALLOWEEN SECRET PETS IN BUBBLE GUM SIMULATOR! \*MUST SEE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dI8thrxKg6E
    * SECRET COSTUME CHEST UPDATE CODES IN HALLOWEEN SIMULATOR! \*SECRET CODES\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=tC9zoAmf0Ss
    * SECRET HALLOWEEN EVENT PET CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=5RkALuFR2UI
    * ALL HALLOWEEN PET UPDATE CODES IN SABER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=2eSj_I5f_zI
    * ALL SECRET HALLOWEEN BOSS CODES IN HALLOWEEN SIMULATOR! \*FREE BILLIONS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dNu-VHFYF5E
    * ALL 18 SECRET OWNER CODES IN SABER SIMULATOR! \*FREE CROWNS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=XpQfQvftvMA
    * ALL 11 SECRET OMEGA PET CODES IN MAGNET SIMULATOR! \*FREE PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Y0jiMwgDGm8
    * 7 SECRET OWNER AURA CODES IN HAMMER SIMULATOR! \*NEED TO TRY THESE!\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=AEpr6iLD8pE
    * ME And MY GIRLFRIEND Adopt A \*FREE\* FLYING PUPPY In ROBLOX ADOPT ME!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=gJ0-uuqC0uQ
    * 15 SECRET OWNER CROWN CODES IN SABER SIMULATOR! \*MUST GET\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4RRSugQCUdM
    * Roblox Restaurant Tycoon 2 BUT I Can't Actually Cook... \*GONE WRONG\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ts3wfsBda08
    * Finding 11 SECRET SPOOKY CODES In MAGNET SIMULATOR! \*YOU NEED THESE!\* (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dLeMahqYO5I
    * Roblox Saber Simulator But Instead You Swing A Hammer.. ITS ACTUALLY GOOD!?
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=_ntB9IhD1tY
    * If I Swing A Saber In Roblox Saber Simulator, The Video Ends..
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=EmgWUGA4gTY
    * ALL 10 SECRET CROWN AND STRENGTH CODES IN SABER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=7XFGGQeikbY
    * UNLOCKING \*MAX\* MAGMA REWARD ISLAND PETS IN BUBBLE GUM SIMULATOR!! \*SUPER OP\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ttATP1cx_F0
    * ALL 28 SECRET GIFTED WINDY BEE UPDATE CODES IN BEE SWARM SIMULATOR! \*BEST BEE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=AMMiGuH8N-Q
    * NEW UNDERWORLD, MAX BUBBLE PASS 2 CODES AND MORE IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Ih699y-KciU
    * PET SIMULATOR 2 RELEASE!? \*ALL YOU NEED TO KNOW!\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=H_hG5WE583o
    * NEW SPECIAL CODES!? 14X BEST SHINY KNIGHT PETS IN MAGNET SIMULATOR! \*MUST HAVE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=CdEqQOFwvI4
    * SECRET REWARD CHEST UPDATE CODES IN MAGNET SIMULATOR UPDATE 18! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=2_bqU6dWS-0
    * I Spent $14,000 ROBUX And I Got.. SCAMMED By LIFTING SIMULATOR?! \*RIP ROBUX\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=C9l_YSF8Ipg
    * 8 SECRET OWNER MONEY CODES IN SABER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=EvgkElDXprs
    * ALL 14 SECRET PET CODES IN MAGNET SIMULATOR!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Gq-X8tL-Srg
    * NEW SPACE EVENT PET CODES IN MAGNET SIMULATOR!! \*FREE PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Pbw8lFFwf7k
    * NOOB VS PRO YOUTUBER VS HACKER IN MAGNET SIMULATOR! Roblox \*Funny\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=la58rczgqGw
    * I FULL TEAM OF SHINY KORBLOX DEATHSPEAKER IN MAGNET SIMULATOR! \*SUPER OP\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=NiM6ihYkIp8
    * ALL MONEY AND PET CODES IN MAGNET SIMULATOR UPDATE 17! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=idbaw6DHDI8
    * HOW TO GET FREE SHINY NINJA PETS IN MAGNET SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=_Wqu26uESvk
    * I GOT 2700.000.000.000.000.000 REBIRTH TOKENS IN MAGNET SIMULATOR \*MOST EVER\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=_2Akohpk9iU
    * 14X BEST NINJA SHINY PET AND REBIRTH SECRETS IN MAGNET SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=cPwVrN-NF_0
    * I GOT ALL SHINY NINJA PETS IN MAGNET SIMULATOR UPDATE 16!! \*BEST PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=JkxF0uzQd1Y
    * FILLING INFINITE BACKPACK AND FREE X2 CASH IN MAGNET SIMULATOR! \*World First\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=A2ZF6BRVbH8
    * FULL TEAM OF BEST SHINY AQUATIC PETS IN MAGNET SIMULATOR! \*BEST PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=P4Uad2ZsZIs
    * NOOB To LIFTING GOD!! MAX POWER, MAX SIZE IN LIFTING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=OrCwFy9GTQ8
    * I BOUGHT The NEW $1,400,000,000,000,000 MAGNET And 1 MILLION REBIRTHS In MAGNET SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ndMXmXDlaks
    * PRANKING YOUTUBERS WITH FIRE! - Minecraft Youtuber Survival \#4
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=A1x5sQZNvY0
    * 7 NEW Skeletons that MINECRAFT Should Add!!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4w096E8OVCM
    * DIAMOND MINING! - Minecraft Youtuber Survival \#3
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=HDEClXz22m8
    * 7 NEW Zombies that MINECRAFT Should Add!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=8kLwTDzOREA
    * NEW YOUTUBERS JOIN! - Minecraft Youtuber Survival \#2
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=wNAqxmEx6kQ
    * Minecraft BUT Chat Decided EVERYTHING I Do... \*BAD IDEA\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=iFJSE_wZHz4
    * SHINY TYCOONIST PETS, MAGNETS AND MORE! IN MAGNET SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=kln6PAbVWC8
    * I GOT BEST LEGENDARY SCYTHE AND GODRAY SPELL IN WIZARD SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=QTlJEh0FWrc
    * NOOB TO MASTER! BEST MAGNETS AND EVERYTHING UNLOCKED! MAGNET SIMULATOR! \*NO ROBUX\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=uRofZQx8Yow
    * NEW BEST HATS AND SEALAND REBIRTH ZONE IN MAGNET SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=YN8Nm54kO1g
    * FREE SHINY LAVA PETS AND BEST DUAL MAGNET IN MAGNET SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=jt_qQaNGmWQ
    * Roblox CAMPING With KIRABERRY! (The Hotel) \*GONE WRONG\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=y3FPGYEbqm8
    * PLAYING BOTTED ROBLOX SIMULATOR GONE WRONG... \*BAD IDEA\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dy9m4DLh_-w
    * ALL SECRET FAIRY UPDATE MONEY CODES IN UNBOXING SIMULATOR! (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=sdOMl1zdVGo
    * GETTING SECRET SHINY PETS IN WIZARD SIMULATOR! \*BEST PETS\* (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Rgke3pElAUo
    * REVIVE UPDATE CODES! Unboxing Simulator \*UPDATES ARE BACK!\* (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Pnxhi7OI9-o
    * How To Get FREE GAMEPASSES In WIZARD SIMULATOR! Roblox \*Giveaway\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=FZDn1Z0PejE
    * NOOB VS PRO VS HACKER IN ROBLOX WIZARD SIMULATOR! (Funny)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Nf_7T9ymLe8
    * ALL SPELLS, MAX LEVEL GEAR AND CODES IN WIZARD SIMULATOR! (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=MsVOp3K7OOs
    * I GOT BEST DRAGON PET AND CODES IN WIZARD SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=8NckbcJUiPg
    * I Went UNDERCOVER As A GIRL And THIS HAPPEND!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Ko3xSPznbpw
    * How To Get A FREE PREMIUM BUBBLE PASS In BUBBLE GUM SIMULATOR! (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=m-m56JsSQNw
    * LEVEL 0 VS MAX LEVEL DRAGON IN DRAGON ADVENTURES! Roblox \*SUPER HUGE!\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=_afZljGIe_0
    * 👏 CODE REVIEW 👏 ALL 21 MONEY CODES In RO GHOUL! Roblox \#CODEWEEK
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=FwAHKPp_0mg
    * 👏 CODE REVIEW 👏 ALL 9 FROST CODES In TOWER DEFENCE SIMULATOR! Roblox \#CODEWEEK
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ee1Uoy8v_gA
    * 👏 CODE REVIEW 👏 ALL 9 HIDDEN CODES In Mad City! Roblox \#CODEWEEK
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Qd2BjyFFPoQ
    * 👏 CODE REVIEW 👏 ALL 26 CODES IN POWER SIMULATOR! Roblox \#CODEWEEK
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=biUusTjl72k
    * 4 STORM EVENT CODES, ALL METEOR SHARD LOCATIONS IN POWER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=PCJx09sTijs
    * USER MADE CATALOG ITEMS, PREMIUM AND MORE IN ROBLOX!! RDC 2019 NEWS
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=3ADL9YQSlxg
    * Roblox Song ♪ "I Unbox Alone" Roblox Parody (Roblox Animation)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=p5motL0fp0c
    * ALL SECRET AREA 51 CODES IN ROBLOX TOWER DEFENCE SIMULATOR!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=S3w7ifJtED0
    * ALL SECRET METEOR Training Areas In POWER SIMULATOR! (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=51pn57UhDw8
    * GIANT METEOR LIVE EVENT IN POWER SIMULATOR! \*NEW AREAS!\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=uQv3_EKss3k
    * ALL 20 SECRET OWNER CODES IN POWER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=NjSN2Kr3VdQ
    * WORLD EVENT AND VILLAIN POWER UPDATE CODES IN POWER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=suBQdohqcG0
    * I Got ABDUCTED By HORROR BEARS In ROBLOX! \*Scary\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=PsGIXx9e6EY
    * SECRET WAY TO GET EASY DUSKIT IN LOOMIAN LEGACY! (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=cv1WWSwibUY
    * I Got NEW MAX LEVEL SENTRY And Became OP In TOWER DEFENCE SIMULATOR! (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=_BpBsLi7_BI
    * FREE STARTERS AND NEW TRADE RESORT UPDATE IN LOOMIAN LEGACY! (Roblox)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=v_cHI0cMTJ0
    * SHINY STARTER LOOMIANS AND FREE GAMEPASES IN LOOMIAN LEGACY! Roblox \*Giveaway\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=_hxcTRrTYAw
    * I SPENT $30.000 ROBUX TO BECOME THE BEST IN POWER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=FoP2MtiD0pM
    * ALL SECRET SUPER POWER CODES IN POWER SIMULATOR! Roblox \*INSANE\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Ay7uMalDUgQ
    * GOLD MINE DUNGEON, POTION AND CODES IN UNBOXING SIMULATOR! Roblox \*SUPER OP\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=IQctedNbgRM
    * FREE GAMEPASSES AND GETTING SHINY LOOMIANS IN LOOMIAN LEGACY! Roblox \*Giveaway\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=_7rSmLOiaPw
    * GETTING GLEAMING LOOMIANS AND FREE EXP SHARE IN LOOMIAN LEGACY! Roblox \*Giveaway\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=yoUTCZ0Q89U
    * TRADE RESORT AND LOOMIAN TYPE CHART IN LOOMIAN LEGACY! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=DE1pF6Ysl8A
    * FANCY OWNER PET UPDATE CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ibos_2TBv8Y
    * 22 SECRETS YOU DIDN'T KNOW ABOUT LOOMIAN LEGACY! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=-DS65EfwiYc
    * SECRET FASTEST WAY TO LEVEL IN LOOMIAN LEGACY! Roblox \*BEST EXP\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=NAwK7MPH-Dg
    * HOW TO GET MYTHICAL DUSKIT AND SHINY LOOMIAN POKEMON IN LOOMIAN LEGACY! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=c1CYr3mi5YM
    * GETTING CORRUPTED LOOMIAN POKEMON AND SHINIES IN LOOMIAN LEGACY! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=rFS7I--_iec
    * DEFEATING FIRST GYM AND EVOLVING SHINY FEVINE IN LOOMIAN LEGACY! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=2WeRGhps8DI
    * WORLDS FIRST SHINY STARTER IN LOOMIAN LEGACY!?! \*SHINY FEVINE\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Nc0SW_lk9K8
    * Find THIS HAT And Get $10,000 ROBUX!! Roblox Unboxing Simulator
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=gByQAHmSw-E
    * Get THIS PET And YOU WIN $10,000 ROBUX! Roblox Bubble Gum Simulator
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=XbYl4ZqVJXQ
    * GETTING OUR FIRST RANKUP! Minecraft SKYBLOCK \#2 With Kiraberry
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ZTElnpJNWd0
    * OUR NEW ADVENTURE! Minecraft SKYBLOCK \#1 /W Kiraberry
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=s7BLlohbeOc
    * 10X RAREST SECRET PATRIOTIC ROBOT PET IN BUBBLE GUM SIMULATOR! \*MAX STARS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=MqgiKZsdIWI
    * SECRET CRYSTAL CANYON UPDATE CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=5ylD40q-QWM
    * RAREST NEON PET AND FREE PET TOYS IN ADOPT ME PETS! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=LdSGbTkiUyk
    * ROBLOX PET SIMULATOR 2!? \*GAME LEAKED\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=EqSF4xQbRJg
    * I PLAYED A BOTTED ROBLOX SIMULATOR AND THIS HAPPENED...!?
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4O20GXzVbFU
    * ALL 32 SECRET OWNER CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Bw7ujL9wQ18
    * BOTS ARE TAKING OVER ROBLOX..!!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=JjIXrcium3w
    * FREE LIMITED PETS AND ALL SUMMER CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=6AOThYqa8bw
    * SUPER SECRET FARMLAND COIN CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=oBDMjwjXa-0
    * 4 SECRET SUMMER UPDATE CODES IN MINING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=kk_aU4D0i-s
    * NEW 4TH OF JULY PETS AND CODES IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=2PjjqvbwEyQ
    * New HAT CHEST Gives FREE MYTHICAL HATS In UNBOXING SIMULATOR!? \*Farmland Update\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=K0BFZNnP_y8
    * BEST MYTHICAL HAT AND 2 FARMLAND HYPE CODES IN UNBOXING SIMULATOR! \*MAX DMG\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ReFmZ8dDdPw
    * GETTING OP REBIRTH ABILITIES AND CODES IN HUNTING SIMULATOR 2! \*INSANE STATS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dCGESWvrShc
    * I Became THE BEST HUNTER In HUNTER SIMULATOR 2!! \*MAX POWERS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=uzXB5Qm7sh4
    * I Got The BEST NEW DRAGON PETS In PET WALKING SIMULATOR! \*INSANE COINS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=wAoi1NLfe8w
    * FROSTBITE GENERAL MAY DROP THIS EXCLUSIVE DEADLY DARK DOMINUS HAT?! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ymnJxHABQVs
    * 6 SECRET CODES MADE ME STRONGEST IN WEIGHT LIFTING SIMULATOR 4! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=TefNsu4tJrU
    * OPENING 4000 CLASSIC EGGS AND GOT THIS IN UNBOXING SIMULATOR!? Roblox \*100B GEMS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=VFJOjQQW_Z0
    * 3 SECRET CLASSIC LAND COIN CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=TXWbQ5DTMtA
    * MEGA CLASSIC LAND PETS AND HATS IN UNBOXING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=DCIl_QNYxAE
    * CLANS UPDATE SOON AND BEST MYTHICAL HAT IN UNBOXING SIMULATOR?! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=yvcLdLmUZCo
    * RAINBOW SUMMER CODES AND RAINBOW DOGCAT IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=X33zdZAtAeY
    * SECRET RAINBOW UPDATE CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=i5eQlQfX0T4
    * LEVELED A MYTHICAL PET TO LEVEL 100,000 THEN THIS HAPPENED.. IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=94vh-UTFet0
    * NINJA TEMPLE UPDATE MONEY CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=By58x5wuo_I
    * GODLY NINJA HAT, 200SP+ DMG AND X1000 EXP IN UNBOXING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=-JSuT0TEGWw
    * I DEFEATED THANOS AND GOT INFINITY SUIT IN SUPERHERO SIMULATOR UPDATE! Roblox \*code\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=KjWnZlBSlVM
    * I GOT SECRET PETS AND MYTHICAL CONE HAT IN UNBOXING SIMULATOR! Roblox \*INSANE LEVELS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=WccVpULAEmo
    * QUEST GEMS AND EGG GAMEPASS UPDATE CODES IN UNBOXING SIMULATOR! ROBLOX
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ANYwugz6m80
    * GETTING MY SECRET LEGENDARY PET 🦄 IN ADOPT ME PETS UPDATE!! Roblox \*SO CUTE\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=PZ0BLimNNOY
    * FULL TEAM OF THE BEST MYTHICAL PETS IN OM NOM SIMULATOR! Roblox \*RIP 6000 ROBUX\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=lNky7NdzfSY
    * 12 MYTHICAL PET UPDATE CODES IN OM NOM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ow9IqEewN80
    * SECRET CONSTRUCTION UPDATE COIN CODES IN UNBOXING SIMULATOR! Roblox \*21 OC COINS!\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zxttJfqRqxU
    * MYTHICAL NINJA PET, GIANT PRESENT AND TRADING?! IN UNBOXING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=t4rUVwyEXNs
    * ALL 20 SECRET UPDATE BOOST CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=YVWY4HqC5C0
    * NEW CONSTRUCTION UPDATE SOON! Roblox Unboxing Simulator
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=EyiHT5uMKXE
    * 3 SECRET RAINFOREST BOOST CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=AiuoKrtkd2s
    * ALL SECRET SPEED CODES IN LEGENDS OF SPEED SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=F6cFwF-uXqs
    * RAINFOREST UPDATE CODES AND PET EGGS LEAKED IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=hThBfbyq2TQ
    * RAINFOREST MISC CHEST AND OP GODLY HAT IN UNBOXING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Qo0Frzw5JXE
    * 400M OVERLOAD UPDATE CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dzIbFa0KaFs
    * RAINFOREST UPDATE LEAK AND MYTHICAL DUCK HAT IN UNBOXING SIMULATOR! Roblox \*2.5 SP DMG!\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=CNmx-9meXAE
    * SECRET HAT DROP POTION CODE AND BEST TOOL IN UNBOXING SIMULTOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=kDMqs_lnzAU
    * SECRET BATHTUB CODES AND X32 BILLION DMG TOOL IN UNBOXING SIMULATOR! \*300SX+ DMG\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=YC3e8Sx-bsI
    * BATHTUB DUNGEON, POTIONS AND LEVEL 1 MILLION HATS IN UNBOXING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=6tkDPgabDZk
    * SECRET LEVEL 10.000+ MYTHICAL PET IN UNBOXING SIMULATOR!? Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=9BpS594j0UQ
    * x100 ENCHANTING, F2P IS NOW OP IN UNBOXING SIMULATOR UPDATE!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=X5Pl7Hb8OqY
    * 11 DESERT UPDATE LEVEL CODES IN TREASURE QUEST SIMULATOR!? Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=XRy0cYITazE
    * BATHTUB DUNGEON UPDATE AND CODE LEAKED IN UNBOXING SIMULATOR!!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=bRo4badaoig
    * NEW MINECRAFT SURVIVAL ADVENTURE! - Minecraft 1.14 NostalgiaSMP \#1
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ASpJdS-RwJs
    * ALL CODES AND HIDDEN LAVA BLADE LOCATION IN TREASURE QUEST SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=H151aUfHipM
    * ALL 24 OWNER CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zxACdW7AMww
    * NEW MYTHICAL PETS AND 3 PYRAMID PARADISE UPDATE CODES IN UNBOXING SIMULATOR!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=c5tT5PBvo1U
    * PYRAMID PARADISE LEAKS AND 3 CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=jT5jbZS2IgM
    * 3 CODES, MAKING ALL BOXES PRESENTS AND THIS HAPPENED.. IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=8hEN0q1bTFU
    * BEST 4X EXP LEVELING GUIDE AND 2 CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=K2AqeYMhSY8
    * 6 SECRET MATRIX BOOST CODES IN UNBOXING SIMULATOR!? Roblox \*150% DMG BOOST\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=CJ2LhG1Mr_g
    * MATRIX LAND BOOST CODE AND 4X EXP IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=BhtNBpM7k38
    * SUMMER SECRET PET EGG UPDATE CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=jGfrt6pJav8
    * BUYING 8000 ROBUX MYTHICAL SWORD IN UNBOXING SIMULATOR!? Roblox \*100QA DMG\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=tyMPtSsHZ9g
    * SECRET TOY BOOST CODES IN UNBOXING SIMULATOR! Roblox \*30 SX COINS!?\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=hyIsIxg9tXc
    * TOY LAND UPDATE CODE AND TOOLS IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=IjEqGAPszuQ
    * ALL 12 SECRET OWNER CODES IN VACUUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=o9fvDTNWzeM
    * ROBLOX NOOB VS PRO VS OWNER - UNBOXING SIMULATOR \*FUNNY\*!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=AOIl51OBHqQ
    * ALL POTIONS CRAFTING RECIPES AND CODES IN UNBOXIN SIMULATOR UPDATE! \*600T DMG\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=yuhKA0N8HEY
    * CRAFTING UPDATE LEAKED, 100T DMG AND MORE IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=GtoRyEY-27Q
    * ALL 20 YOUTUBER CODES AND CRAFTING UPDATE SOON IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=1RwJRR-JCXI
    * I SPEND 1 BILLION GEMS AND GOT THIS... IN UNBOXING SIMULATOR Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Pf4VUT57ro4
    * ALL 36 SECRET OWNER CODES IN UNBOXING SIMULATOR! Roblox \*INSANE BOOSTS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=W1q8TVRDjiM
    * SECRET ICE KINGDOM CODES AND GIANT FROST EGG IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=VR6ZpkGCjZw
    * 8 BEST MYTHICAL HATS MADE ME THE \#1 PLAYER IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=7xzV9DaWwxw
    * 25 PRO PLAYERS VS DUNGEON IN UNBOXING SIMULATOR! \*330 TRILLION DMG\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Kc5VTL4x-oQ
    * 3 SECRET FOODLAND CODES AND 12 TRILLION DAMAGE IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=MwUAz_dEe9g
    * 6 OWNER CODES AND SECRET GRYPHON PET IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=0YDpROZm2oU
    * CLEARING NEW DUNGEON + CODES IN UNBOXING SIMULATOR! Roblox \*THIS IS INSANE\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=n468HhayBvw
    * SECRET DUNGEON UPDATE CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=1Z8W_lJIuKI
    * SECRET PET CODE AND MAGMORAUG BOSS IN GHOST SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ma1rnqA_V5M
    * OWNER GAVE ME INFINITE PETS IN UNBOXING SIMULATOR! \*500+ PETS\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=3BtteIHyndc
    * PHOENIX BOSS, 450.000 GEMS OPENING IN GHOST SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dAPSbZrHM10
    * LEVEL 6000+ MYTHICAL HATS AND PETS IN UNBOXING SIMULATOR! Roblox \*INSANE\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=2BEQfqLJwtY
    * ALL 20 SUPER CODES IN UNBOXING SIMULATOR! Roblox \*SUPER OP\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=VsVTjt2fLEE
    * ALL 7 SECRET HIDDEN MAP CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=hjEEJ_5DpAc
    * I GOT THE BEST GODLY VACUUM AND ALL PETS IN GHOST SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zlJCO9ldwDI
    * OP OWNER 35,000 TRILLION COINS CODE IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4qSTZKJ-RGY
    * SECRET FREE MYTHICAL PET IN GHOST SIMULATOR!? Roblox \*SUPER OP\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=fsD7NrGZ8yo
    * I GOT THE BEST MYTHICAL PETS AND HATS IN UNBOXING SIMULATOR!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=DjrHyRe-bJw
    * SECRET OWNER PET CODE AND OP GODLY PETS IN GHOST SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=i1Jc2u3zXuw
    * I GOT A SECRET "Roblox" EGG HUNT 2019 ROBLOX EGG!? \*INSANE\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=b8U3UBelKZI
    * SECRET BEST WAY TO LEVEL PETS FAST IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=sZ8A77W48No
    * SECRET 1500 TRILLION COINS CODE IN UNBOXING SIMULATOR!? Roblox \*THIS IS CRAZY\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=XgAXwYppL9o
    * BEST WAY TO GET MYTHICAL HATS AND PETS FAST IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Tjjvon0DEMA
    * OWNER GAVE ME SECRET YOUTUBER CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=8fTlm6nFY4A
    * ALL UNBOXING SIMULATOR CODES AND LEVEL 1000 GODLY HAT! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=OvjP13HYkK4
    * NEW CANDY LAND PETS AND 500M CHALLENGE IN UNBOXING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ZZGQ3i7knwI
    * 10 SECRET UPDATE CODES AND GIANT PEARL PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=GHyxgaZwZqU
    * NEW SEA PHOENIX, SECRET PET AND NEW ATLANTIS HATS IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ur-258CjJ4E
    * BUYING FULL TEAM OF BEST PETS IN UNBOXING SIMULATOR! Roblox \*10K ROBUX\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=oO22DRex1xY
    * We Played ROBLOX MINING SIMULATOR AFTER 1 YEAR! \*NOSTALGIA\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ixBvb0aTjsA
    * TURNING NOOBS TO PRO IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=yh8eoJk3vYY
    * I GOT SECRET SHINY EASTER BASKET PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=bgoFTtDeegc
    * SECRET EASTER PET TREATS IN ROBLOX BUBBLE GUM SIMULATOR!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=CCmj2CbtG_k
    * ALL EASTER EVENT EGG LOCATIONS IN ROBLOX BUBBLE GUM SIMULATOR! \*QUICK GUIDE\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=HnoLR4fYTCc
    * SHINY EASTER BUNNY CODES IN ROBLOX BUBBLE GUM SIMULATOR UPDATE!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Lx8NXcL-lf8
    * NEW EASTER BASKET SECRET PET IN ROBLOX BUBBLE GUM SIMULATOR UPDATE!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=l1Jiw1yyPU4
    * HOW TO GET AVENGER EGGS + THANOS EGG IN 10 MINUTES! - Roblox Egg Hunt 2019
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=U9jkP_Nk9E8
    * GIVING THE VIDEO STAR EGG + ALL EGG LOCATIONS IN ROBLOX EGG HUNT 2019!!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=6Bk-2LnlDLc
    * ALL 23 BEE SWARM SIMULATOR CODES! Roblox \*MICRO CONVERTERS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=MXBK7OwUTRM
    * ALL SECRET DRILLING SIMULATOR CODES! Roblox \*FREE PETS!\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=U4nbCuVZNUc
    * 3 SECRET BUBBLE GUM SIMULATOR ATLANTIS PET CODES! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=F56rLn98YK0
    * I GOT BEST ATLANTIS GUARDIAN PET IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Ux5-77M_RS0
    * BUBBLE GUM SIMULATOR SECRET BINGO PET EGG CHALLENGE! Roblox  /w KIRABERRY
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=GTM929EqN0M
    * FREE BEST LEGENDARY PETS GIVEAWAY IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=T2m0kHHPYs0
    * FREE SECRET OBSCURE ENTITY PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=vat_twGjpho
    * ALL PET RANCH SIMULATOR CODES! Roblox \*SECRET SHINY PETS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=nyWb1suDO80
    * NOOB TROLLS PRO WITH 9 \*RAREST\* SHINY KING CRABS IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=NcaQYWHlfDM
    * ALL BUILDING SIMULATOR SECRET CODES! Roblox \*GIANT TOOLS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=skGrahFetk4
    * ROBLOX WORLD // ZERO!! ALL CHEST LOCATIONS + ALPHA ACCESS GIVEAWAY! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Cht8YyawUbY
    * 10 BUBBLE GUM SIMULATOR SECRETS YOU DIDN'T KNOW!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=XIraflv-dGw
    * MIDORIYA (DEKU) VS TOKOYAMI IN ROBLOX! - Boku No Roblox Remasted / My Hero Academia
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=lHW0zS-Ub2M
    * ALL 48 BUBBLE GUM SIMULATOR SECRET PET CODES! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=rREXp2kYRZM
    * ALL BOKU NO ROBLOX REMASTERED CODES! \*FREE\* SECRET QUIRKS!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=CBjQRdr3P6o
    * OWNER GAVE ME LEGENDARY PET SPAWNERS IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=8h9kxYOYBd8
    * SECRET SHINY KING CRAB CODES!! | Roblox Bubble Gum Simulator
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=i9CIz02H8uY
    * SECRET ObscureEntity PET HAT IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=23GyPQX1pSY
    * PET SIMULATOR + POKEMON = PET TRAINER SIMULATOR!? Roblox \*INSANE\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=QuE7pYEZ5kU
    * I GOT BEST LEGENDARY KING CRAB IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=WHpYGseXBBs
    * I GOT THE BEST GODLY PETS IN PET DIGGING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=uLE1JKm4YNA
    * GETTING SECRET PETS + LEGENDARY PET GIVEAWAYS IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=8NxVSHaEfFQ
    * SECRET OWNER PET CODES IN GOD SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=_mkT0zD_VyA
    * 6 SHINY QUEEN OVERLORD CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=psC3-tgqo80
    * SHINY 300M TROPHY PET (MAX LEVEL) IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=gKztxFXGcEw
    * PET SIMULATOR 2 RELEASE!? NEW PETS AND MORE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=onwJsx7Xal0
    * I GOT THE SECRET 300M PET IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=7aOcQA0xY94
    * GETTING THE MOST REBIRTHS POSSIBLE ON COOKIE SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=OoRzQenPMxY
    * DUNGEON QUEST!! FREE SPELLS + WEAPONS GIVEAWAY! - Roblox /w KiraBerry
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=j-apoWKl2cs
    * ROBLOX PIZZA PARTY EVENT! FREE EXCLUSIVE CREATOR PURPLE PARTY AFRO!!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=b_Dz89TbXzw
    * SECRET CODES MADE ME THE BIGGEST BABY ON BABY SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=WbSd2oblbII
    * THESE SECRET CODES MADE ME OP IN RPG WORLD SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=HpBYlT1QO4A
    * BEST WAY TO GET POT O' GOLD PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=3qKBIuZc4tU
    * SHINY POT O' GOLD, LUCKY OVERLORD PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=sp8ojY529WY
    * LEGENDARY LUCKY DOMINUS CODES IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Pbe-9D2x47U
    * NOOB TROLLS PRO WITH 9 SHINY JELLY FISH PETS! BUBBLE GUM SIMULATOR! Roblox \*FUNNY\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=RWNrMBfNXXU
    * THESE BUBBLE GUM SIMULATOR PETS GIVE 10,000 ROBUX! Roblox Battles
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=cNCR2PD2cLc
    * 16 PLAYERS VS HARDCORE WINTER OUTPOST IN DUNGEON QUEST! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4tci4pLmRLs
    * HOW TO GET ALL SECRET PETS IN ROBLOX BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=PjZsNGMZbsI
    * I GOT THE BEST SPELLS IN ROBLOX DUNGEON QUEST! \#1
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=R5bN4fX_2jk
    * I OPENED 10.000 WATER EGGS GOT THESE PETS IN BUBBLE GUM SIMULATOR!? Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=E-CA98I1FAY
    * RAREST JELLY FISH PET CODES IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=-HIdcAk7WV8
    * GET FREE SEA URCHIN PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=IdkkW_7inhc
    * BEST ROBUX DOMINUS PET IN BALLOON SIMULATOR! \*BREAKS GAME\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Cw2HIH4vCBk
    * RAREST SHINY CLASSIC TRIO PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dpqLLPLQx9U
    * SECRET GODLY SPARKLER CODES IN FIREWORK SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=97uuDPPfWoQ
    * LEGENDARY TREASURE CHEST CODES IN PET RANCH SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Bw1_Fa4dpJs
    * LEGENDARY SEA URCHIN BEACH CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=fy5b8YNVNVg
    * LEGENDARY LIGHT SERPENT PET CODES IN PET RANCH SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=B9ZAb_dyAFA
    * LEGENDARY SEA URCHIN AND BEACH HYDRA IN BUBBLE GUM SIMULATOR BEACH UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=rZgGUAb8_ss
    * BEST LEGENDARY PET IN DASHING SIMULATOR MOON UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=pz-41v22Fl4
    * 10 SECRET OWNER BOOST CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=jT0YqgQXnnM
    * RAREST SYLENTLY PET IN BUBBLE GUM SIMULATOR! Roblox \*2.5 MILLION STATS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=O6wGRmAdoU0
    * DOMINUS PET CODES IN SLAYING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=s3_sCGhwsnw
    * ALL SECRET PET CODES IN DASHING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ffA7He_JcCk
    * FREE QUEEN BEE PET IN BUBBLE GUM SIMULATOR! Roblox \*GamePass Giveaway\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=X8L8fUwggjM
    * 5 ENCHANT BOOST CODES IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=38uzCzRC6xM
    * SECRET LAVA PET CODES IN SLAYING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=p-wrAkQqQIA
    * LEGENDARY CHAOS SWORD IN NINJA WIZARD SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=GX16EHMItDs
    * PET ENCHANT UPDATE IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=vCxniIw_eLQ
    * GODMODE AND PRESTIGE PETS IN SLAYING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dPolwOt4JWQ
    * SECRET 4M PET UPDATE CODES IN SLAYING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=gCAZFidqRk8
    * RAREST SHINY VALENTIUM PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=hfsRyf8jUmY
    * SECRET LEGENDARY PET CODES IN SLAYING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=QvyvFgWpdOU
    * RAREST SHINY SOUL HEART PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=eGhDBuWWmvQ
    * ALL CODES + 100 GOLDEN CHESTS IN HOT SAUCE SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=nfzlmBr9n20
    * VALENTINE UPDATE CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=aVyFzgaYLXk
    * GETTING THE BEST RANDOM SHINY PETS IN BLOB SIMULATOR 2! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=evZERi3yenI
    * 6 SECRET LEGENDARY BOOST CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=nw5-EouVhAw
    * SECRET QUIZ GIVES FREE LEGENDARY PETS IN BUBBLE GUM SIMULATOR!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=C-nFgeoRdy0
    * TEAM OF DIAMOND OVERLORDS BREAK BUBBLE GUM SIMULATOR!! Roblox \*INFINITE BLOCKS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=mgAzRbU1EE4
    * FREE SHINY LEGENDARY PET BOOSTERS IN BUBBLE GUM SIMULATOR! Roblox \*Gamepass Giveaways\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ChqBl_-mcUY
    * FREE LEGENDARY BOOST CODES IN ROBLOX BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=kiYZGxNQH3k
    * DIAMOND OVERLORD AND FREE LEGENDARY BOOSTS IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=uCy6llGvHbc
    * OPEN THIS EGG To Win FREE GAMEPASSES In BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Sy4yX76ki_E
    * R.I.P HELICOPTER + CODES IN ROBLOX MAD CITY! \*GAMEPASS GIVEAWAYS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Qq-MsU0Mqfc
    * BUYING THE $1.000.000 ROADSTER IN ROBLOX MAD CITY! \*GAMEPASS GIVEAWAYS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=rL5abc9XrG4
    * FREE ELECTRIC HYDRA DOMINUS IN BUBBLE GUM SIMULATOR!? Roblox \*GamePass Giveaways\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=AijCwOhOH4I
    * UNLIMITED FREE NEW ULTRA BEAST PETS IN NINJA LEGENDS!! \*Roblox\* (Giveaway)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Z93A9lAF90E
* Denis (DenisDaily)
  * Information Collection
    * SIR MEOWS A LOT TAKES OVER ROBLOX!
      * Description references the data collection website growbux.net.
      * URL: https://www.youtube.com/watch?v=dwbGKuIDfxA
    * *HOW TO GET FREE ROBUX... I'm not even kidding*
      * *Description references the data collection website growbux.net.*
      * *URL: https://www.youtube.com/watch?v=7Q80_9-GgmU*
* DfieldMark (OfficialDfield)
  * Information Collection
    * THE $600,000 OAKLEY JUNGLE CHATEAU!!! | Subscriber Tours (Roblox Bloxburg)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=kr8aQLtoEIY
    * SECRET JEWELRY STORE DOOR GLITCH!!! \*OPEN 100%\* (Roblox Jailbreak)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=9h-28A22srE
    * HUNTING MY FANS FOR LARGE BOUNTIES!!! (Roblox Jailbreak)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=O1skXWjG3nU
    * THE REVERSE UBER FAN TROLL!!! \*BUGATTI & MONSTER TRUCK\* (Roblox Jailbreak)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=GUCtw8G1O1I
    * TROLLING CRIMINALS WITH AN $5,000+ USD SPARKLE TIME!!! (Roblox Jailbreak)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=dag81qQCWAg
  * Phishing
    * HACKING A FAN'S ROBLOX ACCOUNT FOR HIS BIRTHDAY!!!
      * URL: https://www.youtube.com/watch?v=_jW4TxRsaFo
    * HACKING A FANS ROBLOX ACCOUNT & GIVING HIM ROBUX!
      * URL: https://www.youtube.com/watch?v=dfZ02nL2x0I
  * Non-Giftcard Robux Giveaways
    * KICKING CRIMINALS BEFORE THEY KILL ME!!! \*TROLL\* (Roblox Jailbreak)
      * Robux giveaway uses a Roblox group using group funds.
      * URL: https://www.youtube.com/watch?v=MrJgNha16DM
    * THE 10,000 ROBUX AUTUMN DESIGNER CONTEST!!! (Roblox Bloxburg)
      * Robux giveaway uses a Roblox group using group funds.
      * URL: https://www.youtube.com/watch?v=LX96A2b_kZE
    * EVERYTIME I DIE, YOU GET 1000 ROBUX!!!
      * Robux giveaway uses a Roblox group using group funds.
      * URL: https://www.youtube.com/watch?v=ZxVu1Q6MPAg
* DimerDillon (TheDimer)
  * Information Collection
    * IM ALWAYS THE GOAT! \[ROBLOX SUPER BLOCKY BALL RACING!\]
      * Description references the data collection website robloxwin.com.
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=ZQ_S3PjcZEw
    * RISKING IT ALL! \[Flee The Facility ROBLOX\]
      * Description references the data collection website robloxwin.com.
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=Z6F6p39b_cM
    * DUMBEST WAY TO WIN IN CBRO LOL! \[ROBLOX CBRO\]
      * Description references the data collection website robloxwin.com.
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=T2dTXGlKF_U
    * ONLY DIMES! \[RNFL 2 HEROES EDITION \#1\] \[ROBLOX\]
      * Description references the data collection website robloxwin.com.
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=G-ChfLOF_YE
  * Other
    * Teleporting Simulator! (FUCK FUCK FUCK)
      * Video, title, and description contain the "F word" 8 times.
      * URL: https://www.youtube.com/watch?v=rh_sr6rEWWI
* Diário do Casal Gamer (VictorNuclear and FernandaTreta)
  * Information Collection
    * COMO GANHAR ROBUX DE GRAÇA NO ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=JtHIjfMpyx0
* EmpireBlox (EmpireBloxOficial)
  * Information Collection
    * PEGUEI O NOVO JETPACK DO JAILBREAK!! E ME ARREPENDI !!
      * Description references the data collection website rbx.farm.
      * URL: https://www.youtube.com/watch?v=o3ys9JKB2ng
    * SEASON 3 MAD CITY !! TUDO SOBRE A NOVA ATUALIZAÇÃO !! CARRO GRÁTIS?? (100 RANK)
      * Contains an ad for the information collection website makerobux.co.
      * URL: https://www.youtube.com/watch?v=nbImW_7VfpQ
    * NOVA ATUALIZAÇÃO MAD CITY COM NOVO MISTÉRIO E TEMPORADA 3 ESTÁ CHEGANDO !!
      * Description references the data collection website rblx.pro.
      * URL: https://www.youtube.com/watch?v=_Lgkxx2N7dY
    * NUNCA FIQUE SOZINHO NO CAMPING !! FINAL ALTERNATIVO EM MORTE !!
      * Contains an ad for the information collection website makerobux.co.
      * URL: https://www.youtube.com/watch?v=N9wMf6vFq2c
    * SOFRI ATAQUE HACKER POR CAUSA DA NOROK1 NO RO-GHOUL !! CUIDADO !!
      * Description references the data collection website rblx.pro.
      * URL: https://www.youtube.com/watch?v=OSOpOahE-eI
    * A NOVA KAGUNE DE 15 MILHÕES DE RC NO RO-GHOUL !! VOU PEGAR?
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=2EyKoWmZYnU
    * ADEUS TATARA !! JÁ FOI DECIDIDO !! RO-GHOUL ROBLOX
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=l5YDUqhM8As
    * COMO PEGAR QUIRK NO BOKU NO ROBLOX  !! "GRÁTIS" ? PEGUEI A MELHOR ?
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=XvSzkCCzDog
    * COMO VENDER SEUS ITENS DE ROBUX NO ROBLOX !!
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=1z1iyJlptqY
    * NOVO HOVERBOARD NA NOVA ATUALIZAÇÃO MAD CITY !! VEJA COMO PEGAR !! -ROBLOX-
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=7r5Hp5FHJkA
    * BUG TUDO DE GRAÇA NO NINJA MASTERS !! ROBLOX
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=aTzx12ao6uQ
    * O ROBLOX REALMENTE PODE ACABAR !! SÓ ASSISTA SE VOCÊ NÃO QUER QUE ISSO ACONTEÇA  !!
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=wNMnS7cAbpk
    * NOVO BUG NO SLAYING SIMULATOR !! MATE QUALQUER BOSS FÁCIL !!! -ROBLOX-
      * Description references the data collection website earnrobux.gg.
      * URL: https://www.youtube.com/watch?v=KCUGGh0OdrE
    * GANHE 1.300.000 DE RC NO RO-GHOUL SEM FAZER NADA !!! (ROBLOX)
      * Description references the data collection website earnrobux.gg.
      * URL: https://www.youtube.com/watch?v=9rOzrF8jLIY
    * ITENS DE GRAÇA NO ROBLOX QUE VOCÊ NÃO SABIA !! CONSIGA AGORA !!
      * Description references the data collection website bloxpoints.com.
      * URL: https://www.youtube.com/watch?v=cqZRVS_e59o
    * NUNCA FAÇA ISSO NO CASH GRAB SIMULATOR !! SE FIZER PERDERÁ MUITO DINHEIRO !! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=dQTmV4wSHmI
    * PEGUEI A FERRAMENTA DE 1.000.000M+ NO CASH GRAB SIMULATOR E GANHEI MUITO DINHEIRO!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=H9c86R_pk84
    * ELE FOI SEQUESTRADO NO JAILBREAK PAGUEI 10 MIL PELO RESGATE !! ~ROBLOX~(FILHOJEFF_PARTE 2)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=uT4mO6-erWA
    * FUJA DO PALHAÇO NO JAILBREAK !!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=5v9mJn3OEg0
    * ESSE  PET DÁ MUITO DINHEIRO NO ZOMBIE ATTACK !! ~ROBLOX~
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=U2wBLz-p_Ik
    * MEU CACHORRO ROUBOU A JOALHERIA NO JAILBREAK !! ~ROBLOX~
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ffWT-Xlt6yk
    * MEU FILHO CRIMINOSO ROUBOU O BANCO NO JAILBREAK !!(JeeffBABY)~ROBLOX~
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=tPpg55xeUQA
    * COMPRAR ROBUX EM GRUPO É ROUBO ??(LENDO COMENTÁRIOS EM VIDEO)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=QgI52fzROD0
    * MEU CACHORRO POLICIAL NO JAILBREAK !!! ~ROBLOX~
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=bLSbM7KnsgA
    * ESSA ARMA DESTRÓI MAPAS INTEIROS !! PERIGOSA!? (ROBLOX)!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=39Yrp62zQqU
    * ESSE CARRO TELETRANSPORTA NO JAILBREAK !! ENTREI NA JOALHERIA COM CARRO!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=QlJjLs7n9So
    * SERVIDOR VIP DE GRAÇA NO ZOMBIE ATTACK !! GANHE DINHEIRO RÁPIDO !(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=J_MS5U3PMd8
    * COMPREI SABRE DE LUZ QUE CUSTA 100.000+ !!!!CASH GRAB SIMULATOR !!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Uy_6CrwWzWc
    * BUG QUE DEIXA MUITO RÁPIDO NO JAILBREAK!!! GANHO DE UMA BUGATTI?? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=j0ymKSKeNWk
    * COMO GANHAR MUITO DINHEIRO E LEVEL NO ZOMBIE ATTACK !!FIQUE RICO !!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=9Zz-ND5jNKI
    * NÃO JULGUE PELA APARÊNCIA !! PODER OCULTO??  ZOMBIE ATTACK!!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=unYnOsDF0cU
    * VOCÊ VAI SER BANIDO SE USOU HACKER NO ❄️SNOW SHOVELING SIMULATOR❄️(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=3V6nuJPXpW8
    * FALEI QUE SOU HACKER PARA CRIADOR DO MAPA SNOW SHOVELING !! DESSA VEZ VOU SER BANIDO?!!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=HkfnKmUf5ZE
    * PEGUEI LEVEL 2800+ NO ZOMBIE ATTACK!! PEGUEI 500 NÍVEIS  EM UMA PARTIDA VEJA COMO!!!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=VFux0NphDgw
    * CORRIDA DE CREEPER NO ROBLOX !!! CUIDADO PARA NÃO ESXPLODIR !!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=4pBo1gaAg6Y
    * QUAL O MAIS FORTE ?? COMPREI TODOS OS PETS DO ZOMBIE ATTACK!! VEJA COMO !!(ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Zj5W0eMKaXM
    * A MAIOR E MAIS BUGADA MONTANHA RUSSA DO ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=HvQ55jaWShM
    * JOALHERIA  DENTRO DA PRISÃO DO JAILBREAK ORIGINAL !!! VOCÊ NUNCA VIU !! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=YRTfaVjRLUw
    * YOUTUBERS NÃO TE ADICIONA NO ROBLOX ??? VEJA VERDADEIRO MOTIVO!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ug8T4qQuMHQ
    * ESSE MAPA FOI ESQUECIDO !! DESTRUI ELE !!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=I5jyWu24b-A
    * PEGUEI LEVEL 2000 NO ZOMBIE ATTACK ROBLOX!! PET NOVO !
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=pxN6wWxWuX0
    * ACABOU DE VEZ ?? PAREI COM HACK!!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=9BerN0ms2Yg
    * COMO NUNCA MORRER NO ZOMBIE ATTACK !! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Vzz0hZ_VamY
    * ÁUDIO PROIBIDO NO ROBLOX!! VOU SER BANIDO??
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=X9s1nbk3MHM
    * ESPADA DE LEVEL 1500 NO ZOMBIE ATTACK ROBLOX !!COMO CONSEGUI?
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=10NEnZaHm5k
    * ESSE CARA PERDEU 1 MILHÃO DE ROBUX NO ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=NjiyxPfF8LI
    * PEGUEI LEVEL 1700+ NO ZOMBIE ATTACK ROBLOX!! COMO CONSEGUI?(APRENDA)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=r3jGPwtXELs
    * VOCÊ CONHECE MESMO O EMPIREBLOX?? (NEM EU SABIA ISSO HAH) (QUIZ ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=0k3H4dsjY28
    * CRISTAL ESCONDIDO NA MONTANHA DO SNOW SHOVELING ☃️!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Ji48tNFglhY
    * JEFFBLOX FOI BANIDO do ROBLOX!! CUIDADO COM ESSE ÁUDIO!!
      * Description references the "black market for limiteds" and information collection website rbx.place.
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=8y9Ayik2I20
    * COMO PASSAR DE NÍVEL RÁPIDO NO ZOMBIE ATTACK ROBLOX!! (ALGUMAS DICAS)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=InAWyRo0UDo
    * PEGUEI LEVEL 1100+ NO ZOMBIE ATTACK !! APRENDA COMO PEGUEI TÃO RÁPIDO!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=X-cOFNaGHtg
    * FAZENDO BURACO NEGRO NO ☃️SNOW SHOVELING SIMULATOR❄️!!! ENGOLIU TODA NEVE?? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=PjPlUI_6S5U
    * PEGUEI LEVEL 300+ NO ZOMBIE ATTACK !!!QUER SABER COmO?? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=bp1rW0RJG3c
    * DESTRUINDO MAPA ORIGINAL DO JAILBREAK !! USEI HACKER ??(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Ua7Qbbn4l6E
    * PEGUEI HELICÓPTERO INVISÍVEL NO JAILBREAK !! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Vd3ldiNkJO4
    * ROBLOX PAROU MUITOS HACKERS !! ESSE É O FIM?? (ATUALIZAÇÃO)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=EIr80GtxZJM
    * MAPA CHEIO DE PRESENTE NO ☃️Snow Shoveling Simulator❄️!!! FIQUEI RICO??(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=3Vt_a6ltrsg
    * OQUE TEM DEPOIS DO TÚNEL ??NOVOS ITENS !!! ☃️Snow Shoveling Simulator❄️ (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=9A1lj52ebLE
    * DESTRUÍ O MAPA DE NEVE INTEIRO !! USEI HACK ??? ☃️Snow Shoveling Simulator❄️(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=li1emjpFHE4
    * O GUEST É MESMO O VILÃO NO ROBLOX?? ESSE VIDEO VAI MUDAR A FORMA QUE VOCÊ PENSA!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=4fXo1XrNhpg
    * COMO USAR QUALQUER DOMINUS NO ROBLOX !! (ADMIN COMANDOS)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Dw61Chbp2Jg
    * HACKEANDO A CONTA DE UM INSCRITO NO ROBLOX !!! (TROLLAGEM)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=isqZkScG-UM
    * MUITO RÁPIDO NO ❄️SNOW SHOVELING SIMULATOR❄️ !!CORRI NA NEVE COMO FLASH !!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ua6Tsjg8e34
    * DEI A SENHA VERDADEIRA DA MINHA CONTA ROBLOX PARA INSCRITOS!!!! ME HACKEARAM ??
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=31IUsQzkf7Y
    * QUASE PERDI O CANAL POR CAUSA DESSA CONTA DO ROBLOX !!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=hk27Y-P52D0
    * TROLLEI JEFFBLOX !! ENTREI NA CONTA DELE E TRANSFORMEI EM MENINA!!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=K_ras1bAJic
    * MANDEI NUDS NO GRUPO !!! ROBLOX (TROLLAGEM)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=o9wN-AAGnLs
    * FIZ HACKER NO GUEST 666 !! DESTRUÍ ELE ??(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=5jhaKkVZpvc
    * PASSEI A SENHA DA MINHA CONTA PARA TODOS !! (TESTE HONESTIDADE  ) ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=JXJyNDrL-3w
    * UM HACKER SOUL WATCH FALOU COMIGO !! QUEREM ME HACKEAR !!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=JiChSy1hJ0U
    * ENSINANDO HACKER JAILBREAK IMORTALIDADE !!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=epA9pBmzsR0
    * NÃO VOU MAIS JOGAR ROBLOX !!?! NÃO DA MAIS ??
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=C_VTNyOq5EU
    * VOU TRAZER O MAIOR HACK DE JAILBREAK DE TODOS!!?! VOCÊ VAI DECIDIR (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=m4eoxMrTXm8
    * COMO PARAR O TREM NO JAILBREAK !!!? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=SJexYuR0AFE
    * O HACKER DE IMORTALIDADE VS O TREM DO JAILBREAK !!! DESTRUÍ ELE ???(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=X5TkPkZAL48
    * OLHA OQUE ACONTECE SE BATER UM CARRO DE FRENTE COM O TREM DO JAILBREAK !! SERÁ QUE EXPLODE??
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=JLFtV7MjYqU
    * 5 ERROS DA NOVA ATUALIZAÇÃO DO JAILBREAK !!! NÃO VOU MAIS ROUBAR !!( ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=8ymgmGYpAFQ
    * CUIDADO !! NOVO TIPO DE HACKERS NO ROBLOX!! ELES QUEREM SUA SENHA !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=XVaHMCFP-Ac
    * NOME REVELADO!! ESSE CARA VAI TE SEQUESTRAR NO ROBLOX ??
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=K4dtJ5kk9ow
    * NOVO COFRE NO JAILBREAK E CORES RARAS QUE NÃO PODE SER COMPRADA NO JAILBREAK !! (ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Nz4_lRevuhI
    * FALEI QUE SOU HACKER PARA CRIADOR DE MAPA BRASILEIRO !!!VAI FICAR BRAVO? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=kzID4fx3uaw
    * FIZ HACKER E O CRIADOR DO MAPA FALOU COMIGO !!! E AGORA?? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=tN_YMqvLF-c
    * VIREI MENINA NO ROBLOX SAIO DO ARMARIO?? (TRAVECO) AHHAHA
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=RtbpeL2YRVg
    * ESSE COFRE ESTA CHEIO DE DINHEIRO NO JAILBREAK DENTRO DO TREM !!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=cNDHBfuLKSc
    * CONFIRMADO!! TREM DO JAILBREAK VAI VIR E PODE SER ROUBADO !! CHEGA ESTA SEMANA !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=1GQi68hFVvc
    * ITEM DE GRAÇA ROBLOX !! PROMO CODE !! VOCÊ SÓ TEM HOJE PARA PEGAR !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=2LCqodo3imE
    * FALEI QUE SOU HACKER PARA O CRIADOR DO JAILBREAK \#2!! ELE FICOU BRAVO ?
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=SvpdqlI0jCE
    * JOGANDO NO NOVO BANCO DO JAILBREAK ??(TESTE) VI O TREM !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=elOass51Hxg
    * USEI HACKER E DESTRUÍ UM MAPA INTEIRO !?? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=gbDm0YKosSU
    * ADICIONEI UM HACKER SOULWATCH NO ROBLOX !!! NUNCA FAÇA ISSO !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ZAvgLCXEH2U
    * NOVO PRESENTE COM ITEM MISTERIOSO !! SAIU SEGUNDO PRESENTE !!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=cY6NOpYtk98
    * NOVOS OBSTÁCULOS NO BANCO DO JAILBREAK !! NÃO VOU MAIS ROUBAR NO BANCO !! (DIFÍCIL)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=r1d-74jigO0
    * O HACKER DE IMORTALIDADE NO JAILBREAK É INÚTIL ?? FANTASMA !! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=dMbsPlwQobo
    * TROLLANDO COM ADMIN COMANDOS \#2 !! TRANSFORMANDO TODOS EM PRINCESAS !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=CTf5o7A7IDs
    * IMORTALIDADE NO JAILBREK !!! VOU FICAR RICO !! 💰💰
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=_GB7dsXGn-U
    * COMO ABDUZIR ALGUÉM COM ADMIN COMANDOS !!! (TROLLAGEM)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Uzd8vRNH5Mg
    * COMO USAR QUALQUER ROUPA COM ADMIN COMANDOS NO ROBLOX !! USEI ROUPA DE 1 BILHÃO !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=n6otRL6jeWU
    * O TREM DO JAILBREAK VAI VIR MESMO NA PRÓXIMA ATUALIZAÇÃO ?? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=e28nnpjReO8
    * MELHOR MOTO DE 1 MILHÃO DO JAILBREAK ??(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=g7Ry2L_8jZ0
    * FUI AMEAÇADO !!! VOU PERDER O CANAL ??
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=D3BQaZkhVpM
    * COMO IRRITAR UM LADRÃO DE ROBUX !!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Mv4PSSN_ctI
    * TROLLANDO COM ADMIN COMANDOS !!! TODO MUNDO ROSA !!(ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=9I6TA5jh4UE
    * ESTÃO ROUBANDO MEUS ROBUX !!! E AGORA?? CUIDADO!!! (ROBLOX)😲😲
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=gagoH9lCU8U
    * VAMOS FAZER DE TODO CARRO UM CARRO DE POILICIA NO JAILBREAK??(ATUALIZAÇÃO)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=IbdG2yl-dUo
    * VOU PARAR DE FALAR EM CHAT DO ROBLOX !!!!! VEJA O PORQUE!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Qu3MdJTdlcQ
    * FIM DOS HACKERS NO ROBLOX NOVAMENTE ??? ADEUS HACKER !!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=capvi_a9uZ0
    * INVADINDO GRUPO DE YOUTUBERS NO DISCORD!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=yX_8VMxSBQY
    * GANHE 100 MILHÕES DE CASH NO ZOO TYCOON COM ESSE CÓDIGO!! CAPTUREI ANIMAIS ÉPICOS !! (roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=AiQlQfqVgcc
    * COMO MATAR MUITO ZUMBI ?! TENTARAM ME MATAR !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=FHhndgF3_H0
    * OS CRIADORES DO ROBLOX VAI TE SUGAR?? MENSAGEM SUBLIMINAR !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=EMvh1EDbW2E
    * SOU INTERESSEIRO DO JAILBREAK ?!!! SÓ ANDO EM CARRO DE 1 MILHÃO!!(TESTE) (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=hso3uYf5EJQ
    * GODENOT É ILLUMINATI ?? GOD NÃO ??? QUE MISTÉRIO SE ENCONTRA NELE!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=l_qDeCm8MFc
    * COLOQUEI MEU NOME DE TRÁS PARA FRENTE NO ROBLOX !!!😲E OLHA OQUE ACONTECEU!! 😲
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ACrtRMsL1To
    * O CARA MAIS NOOB DO NINJA ASSASIN !!! VOU PARAR DE JOGAR ? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=VZCRupB-4kI
    * ROBLOX - NOVO CARRO DO JAILBREAK !!! ESSE CARRO É O MELHOR? (ATUALIZAÇÃO DE INVERNO)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=akMpmEwX2Hc
    * ESSES MAPAS DO ROBLOX PODEM HACKEAR SUA CONTA !! 😲 (CUIDADO)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=hLpgvMONo8w
    * VOU DAR ROBUX  PARA OS INSCRITOS DO CANAL DE GRAÇA !!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=A4vY5d7w1Jo
    * UM CARA ME DEU 500,000 MIL NO LUMBER TYCOON 2 !!É MUITO DINHEIRO??(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=J-DxTtjQDbI
    * 3 COISAS PROIBIDAS NO ROBLOX !! VOCÊ SERÁ BANIDO SE FIZER ISSO !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=_bBlZ4C2pzE
    * QUADRICICLO NO JAILBREAK !! 3 NOVOS CARROS !! (ROBLOX )
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=lWRPeNDf1lA
    * RECUPEREI MINHA CONTA DO ROBLOX QUE FOI BANIDA !!! SAIBA COMO FAZER ISSO!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=zlyWVUJfBIs
    * SORTEIO DA CONTA DO ROBLOX !!O GANHADOR DA CONTA É ...!!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=lSN8-nV8XTE
    * MINHA CONTA FOI BANIDA PERMANENTE DO ROBLOX!! NUNCA FAÇAM ISSO !! 😲
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=qrhaAPZB38A
    * MINHA EX CONTA FOI HACKEADA ? VOU TER QUE FAZER NOVO SORTEIO? (ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=TDKxXYiUa7U
    * TENTEI FUGIR DO MEU LADO SOMBRIO!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=zujJoqX_tTA
    * COMO SER IMORTAL NO GUN SIMULATOR!! AGUENTA MAIS QUE 1 TIRO?? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=hJd2_L2bMv8
    * O NOVO SIMULADOR DE ARMAS NO ROBLOX!! VIREI UM ATIRADOR PRO !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=v4k4FtS8O0k
    * CONFIRMADO!!! VAI NEVAR NO JAILBREAK E TERÁ MUDANÇA NO MAPA!! (IMAGEN) ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=8ZeRA93r2qU
    * ESTÃO TENTANDO HACKEAR A MINHA EX CONTA DO ROBLOX QUE DOEI!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=aHqcaggmWNg
    * O ROBLOX MENTIU PRA VOCÊ SOBRE OS HACKERS !!! (BOMBA)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=czgecm_XA2E
    * O ITEM QUE CUSTA 1 TRILHÃO DE ROBUX NO ROBLOX!! MAIS CARO DE TODOS !
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=evEC69qIwjI
    * VOU DAR MINHA CONTA DO ROBLOX HOJE !!! TODOS TEM CHANCE DE GANHAR!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=DChi_HbIGg0
    * PAPAI NOEL ASSASSINO !!! SANTA SIMULATOR ! NOVO JOGO (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=OW3U48vfwTY
    * DANDO A CONTA DO ROBLOX AGORA NA DESCRIÇÃO !! CHEGUE PRIMEIRO E GANHE!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=uOV6RPFIp9A
    * ACHEI UM CARA COM 29 MILHÕES DE NINJÚTSU NO NINJA ASSASSIN!! (ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=DLVX2x68aZ0
    * NOVO CARRO DO JAILBREAK!! MELHOR QUE A BUGATTI ?? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=pxV08xec8TU
    * O CARA QUE TEM TODOS OS ITENS E DO ROBLOX ?? ELE TEM O ITEM PROIBIDO!!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=AxL0ySIF0cE
    * COMO CONSEGUIR UMA ESPADA NO JAILBREAK OFICIAL  !!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Uu85v5TKS0U
    * O ANTHRO ESTÁ CHEGANDO NO ROBLOX ? (TESTE)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=lqKqqs2hna8
    * SIMULADOR DE KARATÊ NO ROBLOX !! (KARATE  SIMULATOR)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=5NtMaSOmF70
    * TESTE DE HUMILDADE DA MENDIGA NO JAILBREAK! MUITO TRISTE!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=D5oDoHYyI0k
    * COMO USAR ADMIN COMMANDS NO JAILBREAK (CÓPIA) !! ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=rRUZCZwwlCk
    * PEGUEI A BAN HAMMER A MELHOR ARMA DO MAPA !! KNIFE SIMULATOR! ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=352DaLbPTAw
    * CORRIDA DE HOVERBOARD NO JEILBREAK !!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ytjIFISotLE
    * COMO VOAR NO JAILBREAK !!! NOVO MÉTODO !! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=DrnXcKDSh_Q
    * TENHO TODAS KNIFES (FACAS) NO KNIFE SIMULATOR !! ~ROBLOX~
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=odi3WfBdekA
    * FIZ CLONAGEM ?? GUERRA!!!! CLONE TYCOON 2!(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=-FMkx1KufCs
    * VAI TER NEVE NO JAILBREAK ?!! NOVIDADE !! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=CKe8s3af6Nc
    * NOVO MÉTODO DE ATRAVESSAR PAREDE NO JAILBREAK !! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=iPYJAj7zii4
    * O ROBLOX PREJUDICOU MUITAS PESSOAS !!! FIM DOS (HACKERS)??
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=PoVHh8E06M0
    * COMO TER PODER INFINITO !!V NO GOD SIMULATOR 3 ! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=eTOcVyVtu_M
    * O MENDIGO DA BUGATTI NO JAILBREAK \#1 !! (ROBLOX) NOVA SÉRIE
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=_Cl3qEI4rZQ
    * VOU DOAR MINHA CONTA QUANDO PEGAR 18 MIL INSCRITOS !!!ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=o4DT19OXCYw
    * QUER SER YOUTUBER?? VOU DIVULGAR 2 ESCOLHIDOS!! E GANHE ROBUX !(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Zt69rczz7oU
    * PEGUEI 4.000.000M+ NO NINJA ASSASSIN!!! TOU ÉPICO MANOO ! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=5Z3HIwtrSHQ
    * COMO TER DINHEIRO INFINITO NO KNIFE SIMULATOR !! ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=WowqAbZY7Qk
    * VOU DOAR MINHA CONTA QUE GASTEI 22000 MIL DE ROBUX!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ic60G5fwOy0
    * MINHA NOVA INTRO !!! FAÇA UMA CRIATIVA PARA MIM !! QUE AJUDO
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=0uGWOfa0zQU
    * VOCÊS VÃO ME BANIR DO ROBLOX !!! AGORA SIM VOU PERDER MINHA CONTA!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ZrcGnnBNR9E
    * DESAFIEI O DONO DO ROBLOX FALEI QUE SOU HACKER !!! AGORA SIM BRINQUEI COM FOGO !!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=-2_mJMjf3Mc
    * DESAFIEI O CRIADOR DO JAILBREAK FALEI QUE SOU HACKER !!! VOU SER BANIDO??(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=rGXylDEOkHU
    * O SIMULADOR DE PINGUIM NO ROBLOX COM OS INSCRITOS !!( Penguin Simulator)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=a54cxVsbaIc
    * FIZ HACKER NA FRENTE DO CRIADOR DO MAPA (ROBLOX) !!!! E OLHA OQUE ACONTECEU!!!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=mIjP3DAnptc
    * ROBLOX - DEU TRETA NO NINJA TYCOON !! SE TORNE UM MESTRE NINJA!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=FC1o43tACPE
    * COMO TER DINHEIRO INFINITO NO CARTOON TYCOON !! MUITO FACIL!! ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=dRkm2GWxL1Q
    * TORNEIO DOS MILHÕES COM OS INSCRITOS NO NINJA ASSASSIN !! QUEM VENCEU??
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ky6kAK3do34
    * TUTORIAL PARA INICIANTES NO SWORD BURST 2 \#1 !! RUMO LV 40 HAHAH !!! ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=SEIZNxsaobk
    * TORNEIO COM OS INSCRITOS NO NINJA ASSASSIN !! PERDI PARA MEUS INSCRITOS?? !! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ib0HWYgJkLc
    * REVELANDO SEGREDO DE COMO PEGUEI 3 MILHÕES TÃO RÁPIDO NO NINJA ASSASSIN !! USO HACK?? (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=1IDhhaI9ptk
    * SERVIDOR VIP DE GRAÇA NO NINJA ASSASSIN!! (ROBLOX) PEGUE MUITO NINJUTSU!!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Sklt1EL8JcY
    * COMO GANHAR MUITO ROBUX DE GRAÇA EM ALGUNS MINUTOS!!! (COMPROVADO) 100%
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=vHZse_X4V-w
    * COMO PEGAR VELOCIDADE INFINITA NO SPRINTING SIMULATOR 2 !!! PEGUE 1 MILHÂO !!! ROBLOX
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=WbE_hz87_Ms
    * PEGUEI 3 MILHÕES  DE NINJUTSU NO NINJA ASSASSIN !! MINHA NOVA AURA !!! (ROBLOX)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=fQl60B3lBHQ
  * Other
    * LOCAL DA CHAVE COPPER CROWN REVELADO?!?! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=VyrABQtYsV4
    * NO JAILBREAK EXISTEM QUANTAS ÁRVORES?? DESAFIO IMPOSSÍVEL!?!!  (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=nYVEV_hWCCY
    * O MASCARADO NO JAILBREAK !! VOCÊ PODE SER O PRÓXIMO!!(ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=xKDaRGUjaKQ
    * TODOS FORAM EXCLUÍDOS  !!! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=oqH3cnTo9bQ
    * ENCONTREI OVO DE ALIENÍGENA NO JAILBREAK !!? ELE VAI ABRIR ? \#TEORIA (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=prkN23KB7rU
    * CUIDADO COM AS MENINAS ASSASSINAS DO JAILBREAK !! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=joVFaP8GhSs
    * CUIDADO JOGADORES DE JAILBREAK !!ELES ESTÃO CHEGANDO!! INVASÃO (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=lmyuWNE7FUc
    * NOVO CÓDIGO DE DINHEIRO NO GHOST BUSTER !! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=9fLKhUWTK0M
    * SIMULADOR DE CAÇA FANTASMA NO ROBLOX!!
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=dMW9VRlF44I
    * NOVO BUG JAILBREAK !! CONSIGO VOAR POR BAIXO??! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=GyGDKsBhb30
    * TÚNEL SECRETO NO JAILBREAK!!! TEM DINHEIRO???! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=QxEsk-1gqF8
    * ELE PEGOU MEU GRUPO DO ROLOX??!
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=OYo0e9WcNO4
    * O BISCOITO ASSASSINO CHEGOU NO JAILBREAK !!!(ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=ytPP5pBCJ0w
    * JAILBREAK VAI CAIR NOVAMENTE ?!! PIOR ATUALIZAÇÃO ?!! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=EoXE-O4CR68
    * VOCÊ VAI GANHAR 45 ITENS DE GRAÇA NO ROBLOX!!! (MELHOR EVENTO)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=a1fWbsZL2T8
    * REVELANDO MOTIVO POR CRIADOR DO JAILBREAK ASIMO TER SIDO BANIDO!!!
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=qauZNGiSgeY
    * MÁGICO NO JAILBREAK !! FAZ APARECER DINHEIRO?!! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=wAWrS6RGlLc
    * 2 BILHÕES EM 10 MINUTOS COM ESSES DOIS ITEMS NO TREASURE HUNT SIMULATOR !!!(ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=5MdIgkc4PMY
    * MISTÉRIO DA NOVA AREIA DO TREASURE HUNT SIMULATOR AREIA 666 !!!!! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=DhLABO9NjL4
    * BUG QUE FUNCIONA NO TREASURE HUT SIMULATOR!! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=2ftR--MX6J8
    * NOVA AREIA DO TREASURE HUNT SIMULATOR !!! QUE COR É ? (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=RptWWRwwBOA
    * CRIADOR DO JAILBREAK FOI BANIDO (ASIMO) !!! FOI UM HACKER? (TWITTER)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=z38aA8vUsLk
    * NOVO ITEM ME DEU 1 BILHÃO NO TREASURE HUNT SIMULATOR !! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=8GdNG61RUYM
    * NOVA ATUALIZAÇÃO TREASURE HUNT SIMULATOR!! PET QUE DÁ MUITO DINHEIRO?!! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=StA5Bc4-NVY
    * PET DE 200 MILHÕES NO TREASURE HUNT SIMULATOR !!! COMO É TELO ?!!(ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=eS0O6afNAFk
    * CAPACETE DE 1000 MOEDAS DE OURO NO BOOGA BOOGA !! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=THbiuUAcpew
    * JAILBREAK VAI VOLTAR ?? OU CAIRÁ NOVAMENTE !!?? (ROBLOX) \#EMPIRENOTICIA!!
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=4Sw0Kye-QpU
    * COMO CONSEGUIR MUITA MOEDA DE OURO NO BOOGA BOOGA !!! ROBLOX
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=5tJZ_DRrkVQ
    * NOVIDADES DA NOVA ATUALIZAÇÃO DO JAILBREAK !!! PIOR ATUALIZAÇÃO?? (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=q40A2O-vfNs
    * MEGA ATUALIZAÇÃO !!! COMO FAZER OURO NO BOOGA BOOGA !!! ROBLOX
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=36nWxc2Q424
    * CARA ESTRANHO DANDO DINHEIRO NO JAILBREAK!!!(ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=4y1B_pobPKQ
    * CARA ENCONTRADO CONGELADO !!! ESTÁ VIVO?? ROBLOX (BOOGA BOOGA)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=md6dOoRYxcA
    * FERRAMENTAS MAIS CARAS PERDIDAS NO MAPA!!! BOOGA BOOGA (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=-AMuYEOfibQ
    * COMO PASSAR DE NÍVEL RÁPIDO NO BOOGA BOOGA ROBLOX!!! \#1
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=EUyw_gyoMNw
    * NOVO BUG DE DINHEIRO NO JAILBREAK !! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=IFTtqpBT08M
    * LUGAR SECRETO QUE DÁ MUITO XP NO BOOGA BOOGA !!! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=RvHZUP3hC54
    * CUIDADO COM A PAREDE MISTERIOSA NO MINING SIMULATOR !!(ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=d6rRj0_NLAY
    * A PEDRA AMARELA 666!!!! MINING SIMULATOR(ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=g-VkGKzwsng
    * COMA ISSO E MORRA !!! BOOGA BOOGA !! (ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=iAblAUIEjss
    * ISSO PODE DESTRUIR UM MAPA DO ROBLOX?? (NUKE REAL OU FAKE) MINING SIMULATOR!!
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=Kyr5nmlENhA
    * BUG DE COMO TER MUITO DINHEIRO NO MINING SIMULATOR !!(ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=b9_wExSt9KI
    * COMO TER ILHA PRIVADA DE GRAÇA NO TREASURE HUN SIMULATOR!!(CHEAT?)~ROBLOX~
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=OhVgAnkJ7oI
    * COMO GANHAR MUITO DINHEIRO RÁPIDO NO MINING SIMULATOR!!(ROBLOX)
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=ameLwuP4Uvc
* FunkySquadHD (0UseCode_Funky)
  * Information Collection
    * I GOT ON THE LEADERBOARD! North Pole Simulator 🎅
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=tI_QWeQh-Aw
    * \*SPENDING\* 16 BILLION GEMS! Bubble Gum Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=z2EKKGdMwPU
    * booga booga.exe has stopped working.
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=MR3XTxfPUB0
    * Giving A Noob A Dark Matter DOMINUS HUGE | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=49UuyDB69n4
    * I GOT THE BIG CAT FOR FREE! Pet Simulator | Roblox (Level 10 Billion)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=QcwsTWCngG8
    * MAKING THE BIGGEST BUBBLE! Bubble Gum Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=PaoHut5RWnA
    * SPENDING 120k TOKENS! Super Power Training Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=3K9dFPlAxC4
    * Donating 60,000 TOKENS TO NOOB | Super Power Training Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=_dY7_z8m19Q
    * \*NEW\* Overpowered BIG CAT Pet! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=3at4aP_Kwyk
    * 100,000 Subscriber Special (QnA)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Jw64220CcTY
    * \*NEW\* Dark Matter Pets! Pet Simulator Update 11 | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Hw2t-rMXFUQ
    * BOSSES on Pet Simulator? (Boss Arena Idea) Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=u2xXAlSwWtY
    * Pro Helps Noob (Noobs Get OP Pets!) Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=h7crUv-Pz5I
    * Making Noob Pets Into RAINBOW Pets! Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=uAIH-EwiarY
    * Most Overpowered Rainbow Dominus Huge! Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ep2Q7LufogE
    * Crafting RAINBOW HYDRA On Pet Simulator (Update 10) Roblox!
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=bhXr_vxR-cU
    * \*NEW\* Pet Simulator Update! UPDATE 10 (Leaked Info)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Sqcd6sJMkEc
    * \*NEW\* Code On Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=O1wQxD8xcrY
    * NOOB BEATS PET SIMULATOR IN 2 MINUTES!  Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=XpEVz_lNkWI
    * I GAVE A NOOB A GOLD DOMINUS HUGE! Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=jAPOD18RNd4
    * 8 Gold Dominus Huge VS Gold Chest! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=DlGZBy1oQyU
    * SHE GAVE ME TWO DOMINUS HUGE!  Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=cShY_VK-T7c
    * \*INSANE\* Jump Glitch on Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=7q44qS0xv50
    * NOOB Gets FREE Dominus Electric! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=wnysl1w4Izo
    * Quest Update Coming To Pet Simulator! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ZaPgRmaYywI
    * INSANE Mega Shark On Build A Boat (it moves!) Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=yWn5Hymo9oU
    * Disguise Trolling On Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=00EdpyS0GDA
    * I GOT THE RAINBOW DOMINUS ELECTRIC! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=1ox_6L4KXwg
    * DESTROYING The Dominus Chest! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=g813mtgPrKw
    * MOST \*Overpowered\* Pet on Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=APrDEwY-qp0
    * Rthro On Pet Simulator! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=8LjNEo3dDlk
    * HE GAVE ME THE SKELETON GHOST! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=RXdTHBcH7ak
    * Catching Scammers! Pet Simulator | Roblox \#4 (scammer takes RAINBOW CORE SHOCK)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=AaTwYoHi3rE
    * Trolling Noobs! Pet Simulator | Roblox (Funny Fail)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=mOasZqeawyY
    * HAPPIEST NOOB GETS MEGA CHEST! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=WR79HJ4ymBo
    * NEW Halloween Update! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=_3z5xhJ6Tdw
    * Catching Scammers! Pet Simulator | Roblox \#3 (meanest scammer!)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=CkLI6ZtNIqg
    * Trolling Noobs On Pet Simulator! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=PC-kfNigMeQ
    * Booga Booga MASSIVE Update Coming Soon! Deep Oceans, New Sharks and MORE! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=JvHwr83NOmQ
    * Most Overpowered Rainbow Shock CORE! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=OKhziBdKIYU
    * Overpowered Tier 1 Pet vs MEGA CHEST! Pet Simulator Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Zr10cnX1EI0
    * Overpowered Tier 1 Pet! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Ld8bnjv-0tc
    * NEW Pet Simulator Update! Hats, Multi Delete and New Shop! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Qd7vZexscrE
    * Catching Scammers! Pet Simulator | Roblox \#2 (scammed twice?)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ntFNgyqHao8
    * This Skip Feature Needs To Be Added! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=l0NSEXKuz78
    * This Chest Blew Up The Map! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=cmWKkqBVVXA
    * Catching Scammers! Pet Simulator | Roblox \#1
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Zzm4YlSEziQ
    * How To Gain FAST XP on Super Power Training Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=0A1A5c_nLBk
    * This Game Is Better Than Phantom Forces! Roblox Fray
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=hPcj6hOHij8
    * NOOB GETS RAINBOW CORE! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=_jnF9mNpjCU
    * \*INSANE\* New Volcano Coming To Jailbreak! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=qO2nH9ZtT-Y
    * Crafting Pets Update (idea) Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Rcqq9X3tEKU
    * OVERPOWERED Weapon On FRAY | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=sU2g6sYZdQs
    * New Destruction Simulator CODE! ROBLOX (5 free levels)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=xSsocg36AN0
    * \*Insane\* 1st Person Roblox Shooter Game! FRAY
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ZSlwA1vceTA
    * PRO GIVES ME RAREST PET! Pet Simulator | Roblox (part 2)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=x2wlcN7Ps4w
    * I GOT THE RAREST CODE! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ytns3F96RWQ
    * \*INSANE\* Glowing PET! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Ylohw3xClbY
    * TRAP ROULETTE on Fortnite Battle Royale!
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=3ps71B2Uj8E
    * PRO HELPS NOOB! Noobs Get New Cyborg Pets | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=8sLRg9YmJc8
    * \*NEW\* Tier Cyborg PETS! Pet Simulator Update | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=BE_67GLo5uo
    * \*NEW\* Dragon Simulator CODE! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=a7a8DCYUeWY
    * Giving Rarest Pets To Noobs! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=yFOTBtHyjB8
    * I GOT THE RAREST PET! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=O8_1TcSsDVo
    * NEW WORKING CODES ON DRAGON SIMULATOR! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=X1TbhyMvGVg
    * INSANE Booga Booga Hacked Server! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=zoDjpGiC0SA
    * GIVING Noobs Gold Pets! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=6nZKJbw7hSI
    * \*NEW\* Destruction Simulator CODE! ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=JGd3mm6Bb5s
    * SECRET AREA on Bee Swarm Simulator! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=LD-0bdFBTiI
    * Exploiter on Booga Booga! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=A3uJPHgfL70
    * \*NEW\* Bee Swarm Simulator CODE! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=hFpTGqk58R4
    * \*NEW\* Booga Booga Update Coming! Halloween Edition?! ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=oVqx8cLx0v0
    * BUYING ALL GAMEPASSES | Garbage Simulator! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=dCJYTncBX1M
    * GIVING A NOOB $100,000 on Bloxburg | ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=bMTDp_f2LWc
    * New Destruction Simulator CODE! ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=7vgOUVEcxCU
    * Yup.. I'm quitting roblox (i hate it)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=CTT4sztCkuM
    * PLAYING DOOMSPIRE on Destruction Simulator! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=2GDXInTcogo
    * PRO GIVES RAREST PET | Pet Simulator! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=DJBigPBtTCM
    * BOOGA BOOGA 2 RELEASING SOON! (ROBLOX ISLANDERS)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=iYgfs5o_c5s
    * NEW Mining Simulator Code!  Roblox \*2018\*
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=5IT9WqQU-FI
    * \*2018\* NEW Working Code On Destruction Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=PPZVTB_-A40
    * NEW WORKING CODE on Destruction Simulator 💥 Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=e2sui1gKDnk
    * PRO GIVES ME RAREST PET! Pet Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=eDnVm-CmNro
    * OWNER Giving Away Blackhole Gamepass?! Destruction Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=sfW3QBSEwME
    * So This Happened... Destruction Simulator | Roblox \*buying blackhole\*
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=JS6CJUtUFO4
    * NEW Exclusive Codes Coming? Destruction Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=d_YalI9OAtQ
    * \*NEW\* Minigun Coming To Build A Boat! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=pXcVi4tGlM0
    * Noob VS Pro VS Exploiter! 💥Destruction Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=qnoyrUWWNYc
    * ALL WORKING CODES on Destruction Simulator 💥 Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=UDWLnlA6riA
    * CUBE leaves FIRST SYMBOL! Fortnite Battle Royal | \*easter egg\*
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=E8W9y-yc0Ac
    * Minecraft Hotbar In Jailbreak! (2018)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=cl1DiIJuf3s
    * THE CUBE ACTIVATED! Fortnite Battle Royal | \*cube turns on!\*
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=iL35pf7WlMA
    * CUBE Going To Salty Springs? Fortnite Battle Royal | \*CUBE MOVES ON VIDEO!\*
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=yaeesNOsKAs
    * THE CUBE IS MOVING! Fortnite Battle Royal | \*EVENT\*
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=4x-64oGHCGY
    * RIFT SPAWNS CUBE! Fortnite Battle Royal | New Easter EGG!
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Yzo849IxD50
    * Undercover Noob Ep.11 Booga Booga | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=kd_BaB_ZEzQ
    * Ghillie Suit Trolling! Roblox Jailbreak
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=YHDjaHNlwz0
    * OWNER vs EXPLOITER! Roblox Jailbreak
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=5cCj_oGp4Dk
    * Building A Archery ROOM! Roblox Rovive \*MEGA TOWER\*
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=FQZlkG8ZYQQ
    * Roblox | Rovive Funny Moments! GET ON THE GROUND!!
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=-9j__AWU964
    * BIGGEST BASE RAID! ROVIVE | Roblox \*huge explosion\*
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=G33VYBI7b7Y
    * BEST Survival Game On Roblox! Rovive
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=lNr5QF0y2WU
    * HOW TO GET THE BOSS GAMEPASS FREE! Roblox Jailbreak
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=WnZJMEcupOU
    * How Much Robux Did Asimo & Badcc Make On The BOSS UPDATE?! Jailbreak
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=akg14bklqac
    * NEW Update On Jailbreak! BOSS UPDATE! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=4yzqfvQQ8Qs
    * Booga Booga 1v1 BATTLE (WINNER GETS DOMINUS!) ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ZTK-m0CIOPE
    * SECRET ITEM COMING IN THE NEXT UPDATE?! Roblox Jailbreak \*secret weapon?\*
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=zE07X20QFcg
    * Jailbreak Voice Trolling | ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=LwktqGF7XvA
    * RIDING SHARKS ON BUILD A BOAT! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=zLOvi3YtpZ0
    * Secret Teleporters On Build A Boat! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Mn2qCQLf_LE
    * \*CRAZY\* MEGA WARSHIP! Build A Boat For Treasure | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=mz6xdCR1n2Q
    * \*INSANE\* Maze On Build A Boat | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=mFJxUqATie4
    * GEARS ON BUILD A BOAT!
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=6rW9JULjhdY
    * \*NEW\* Build A Boat DEATH RUN! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=r6-IwF1oFUw
    * CRAZY Glitch To End! Build A boat For Treasure | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=cljfmDGXor0
    * \*CRAZY\* DEATH RUN on Build A Boat | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=5rEflLOOxWs
    * Booga Booga Airdrops? Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=JTPXjmoKjBY
    * Booga Booga With Guns?! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ILMaimHKbnQ
    * Building My Real Life House On Roblox! Part 1
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=hndO5sfNnsU
    * ADOPTING MY FIRST CHILD!! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ERTWXq1TjMI
    * NEW BOOGA BOOGA GAME! COMING SOON | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=84kTJXxa6ME
    * CHICKEN BOSS! Egg Farm Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=P0mOdH8v6TI
    * VIP IS OVERPOWERED! Egg Farming Simulator | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=JMPYVtrzzSA
    * Undercover Noob Ep.10 Booga Booga | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=t-t-k278e8s
    * I Snuck Into Her Bloxburg House And This Happened...
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=vIhmfuz4o3E
    * Dragon VS Boat (Build A Boat For Treasure) Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ALSaWK9WAMg
    * More Info On Booga Lands! Soybeens New Game
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=LRJvuPDv6aQ
    * Bringing a NOOB Into The Void Dimension! (Ep.3)Booga Booga Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=61OMLEa6bUw
    * New Booga Booga Game? Soybeen Announced A New Game (Booga Lands)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=TdsEAsgCCI8
    * MOST RARE WEAPON ON BOOGA BOOGA! Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=fwgwGFTQkmA
    * Killing Players With Domestic Boi! Ep.2 Booga Booga Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=hffkcFlbz8c
    * Bringing a NOOBS Into The Void Dimension! (Ep.2)Booga Booga Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Em7ixfF5T2c
    * Undercover Noob Ep.9 Booga Booga | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=rWtJxn2aUBU
    * TRAPPING NOOBS WITH MAGNETITE WALLS! Booga Booga Trolling | Roblox
      * Description references the data collection website robuxplanet.com.
      * URL: https://www.youtube.com/watch?v=NuMaoDcvKcw
    * Only 1% of people know about the secret to Frosty! Snow Shoveling Simulator❄️Roblox
      * Description references the data collection website robux.network.
      * URL: https://www.youtube.com/watch?v=-rVsn83x9DE
  * Non-Giftcard Robux Giveaways
    * \*NEW\* Building Simulator Update! Update coming Tonight! $R1,500 Giveaway (READ DESCRIPTION)
      * Link references a Robux giveaway via Discord.
      * URL: https://www.youtube.com/watch?v=1i9mmRDFFFU
* GamingMermaid (Aquaerria)
  * Phishing
    * I HACKED A ROBLOX ODER | Roblox High School Dorm Life | Roblox ODer Trolling
      * URL: https://www.youtube.com/watch?v=XdMWvYf4Rjk
* Geko97 - Roblox (Flexer97YT)
  * Information Collection
    * NUEVA FORMA de CONSEGUIR ROBUX GRATIS 🤑 \[Roblox\]
      * Description references the data collection website rbxcash.com.
      * URL: https://www.youtube.com/watch?v=A_NIUghaUM0
    * EL SECRETO QUE NADIE SABIA de JAILBREAK 😱 \[Roblox\] \[Historia Inventada\]
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=5Lt02hfeNbw
    * POR FAVOR NUNCA ENTRES a ESTE JUEGO de ROBLOX 😰
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=lVRkb90j_go
    * CUIDADO CON ESTE ZOMBIE QUE TE MATA EN ROBLOX 😭 \[Roleplay\]
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=l3qwVIOIwgc
* iamSanna (notiamsanna)
  * Non-Giftcard Robux Giveaways
    * WIN 10,000 ROBUX In This \*NEW\* Adopt Me FASHION CONTEST! (Adopt Me)
      * Uses group funds to give away Robux for a competition.
      * URL: https://www.youtube.com/watch?v=L7_cIHzhFpw
* InquisitorMaster (inquisitormaster)
  * Phishing
    * I HACKED A FAN AND CAN'T BELIEVE WHAT I SAW! | Roblox Social Experiment
      * URL: https://www.youtube.com/watch?v=E0JQBfEbCGQ
  * Non-Giftcard Robux Giveaways
    * WHEN A STRANGER FINDS 1,000 ROBUX | Roblox Social Experiment
      * Randomly gives out Robux using a Roblox group using group funds.
      * URL: https://www.youtube.com/watch?v=wgXTsve1zOs
* IntelPlayz (UseCode_IntelPlayz)
  * Information Collection
    * THE PORTAL FINALLY OPENED | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=PRkqgIxiiUk
    * UPDATE LEAKS! Underworld & More | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=iVl8nkLztVk
    * Playing with MODS on Roblox...
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=LpgOWjG31uA
    * THE OWNER GAVE ME 4 MILLION SILVER | Snowman Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=6U6abz3HeJ0
    * ALL CODES & DEVELOPER GAVE ME EVERYTHING | Fishing Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=JwE32cBhwk8
    * SPENDING ALL MY GOLD TO UPGRADE MY RAREST PET! | Snowman Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=mmGWgi-Vblc
    * WHATS ON TOP OF THE SECRET ISLAND | Snowman Simulator ☃️
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=HCQIR9xRsrE
    * NEW UPDATE SNOW MAP & MYTHIC ZONE! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=bRBa-5goNg4
    * I BOUGHT 3 MILLION SILVER THEN GOT MYTHIC PRESENTS | Snowman Simulator ☃️
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Zw4IjLUFJ7A
    * ALL CODES! | Battle Bot Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=qvAvo7ly6Wo
    * A FAN GAVE ME THE RAREST PET | Bubble Gum Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Ehn7PDDLbH8
    * ULTIMATE TIER PETS (Elf OP) | North Pole Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=MMN0_fpA5No
    * MAXING OUT MY ARMY | Army Control Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=XYf_lJ7EshU
    * I GOT THE RAREST GODLY PET (update) | North Pole Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=onK_XjgMpZo
    * I GOT THE MOST POWERFUL SPELL & THE INF BAG! | Army Control Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=iyOJKxdNvIs
    * I BOUGHT THE BEST SLEIGH AND BROKE THE GAME! | North Pole Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=j-OHtLszPZM
    * I BOUGHT THE MOST EXPENSIVE GAMEPASS (5k R$) | North Pole Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=DiFJ73lsgxA
    * THIS CODE GIVES YOU THE BEST RANK! | North Pole Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=NffuVMaCqFc
    * I BOUGHT EVERY GAMEPASS! | North Pole Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=UzxC0Eh4H-A
    * GREAT GOLDEN SWORD (op)! | Army Control Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=-mLDQL0I6QI
    * SHINY PETS AND GEM GENIE  UPDATE!!  | Bubble Gum Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=8EoDCx1TVos
    * THE OWNER GAVE ME THIS..... (OP) | North Pole Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=WFFwfnmkIjY
    * THE OWNER GAVE ME UNLIMITED SPINS | Slot Machine Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=_85tzNOWE7o
    * FIRST LOOK! (Snow Shoveling Simulator 2) | The North Pole
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=6hQ2JP8pzFU
    * REBIRTHING WITH ONLY THE HOT DOG STAND! (Challenge) | Billionaire Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=HB08FtH6Ev0
    * BUYING THE MOST EXPENSIVE FLAVOR & MONSTER EGGS! | Bubble Gum Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=sjllhasdkVA
    * MAKING THE CITY GOLD! | Billionaire Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=u-HBx87800Q
    * IntelPlayz - cLout chasin (Lacrase Diss Track) Official Video
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Sx47-eLJI0c
    * MAKING MILLIONS IN 10 MINUTES |  💰Billionaire Simulator💰
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=0zVTZj342-Q
    * HE MADE A DISSTRACK ON ME (Reaction)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ZwR0ub0qmJo
    * WE GOT 5 GIANT CATS IN 1 SERVER (Insane) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=t6JQCQpUlmw
    * HOW I GOT 1 BILLION COINS AND ON THE LEADERBORED! | Bubble Gum Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=iyheovxKkxw
    * Noob vs Hacker vs Pro \[ Bubble Gum Simulator Edition\]
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=P_KSsgXTLVQ
    * \*NEW\* VOID AREA & EGGS | Bubble Gum Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=k0v4gqkwNhQ
    * I GOT THE GIANT CAT | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=vR9RwO37unw
    * NEW GAMEPASSES & DARK MATER EGGS! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Gz_QZV7iMmA
    * BUYING GAMEPASSES TO UNLOCK THE BEST FLAVOR | Bubble Gum Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=x7BLtu5273A
    * CREATING A GIANT PENGUIN! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=JA8rR5LLRYg
    * RAINBOW DARK MATTER PETS! (Glitched) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=OZPAQgr3I50
    * THE RAREST PET IN THE GAME DARK MATTER AGONY | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=26knAsuRkZo
    * GATHERING SLIME TO GET TO THE TOP! | Slimeulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=BnTX9BHca4o
    * HOW TO GET THE GIANT CAT! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=A6JHy6k5WkE
    * MAKING DARK MATTER DOMINUS HUGE (Bad Idea) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=jN2jbIi1QTk
    * WE USED 1200 DOGS AND BROKE THE GAME! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=wo4ApuvPd58
    * I GAVE HIM THE DARK MATTER DOMINUS HUGE | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=8n3BxHm2Shw
    * \*NEW\* DARK MATTER PETS AND LIMITED TIME RAINBOW EGG | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=RScOAp5EQZ4
    * I BOUGHT 3 OP GAMEPASESS | Prison Escape Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=2FCdoTeeLrI
    * I WILL BE THE BEST BLOB BUSTER | Blob Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=gEVv8XA5vM8
    * BROKEN MONEY ACCOUNT GIVEAWAY | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=vArbaApnV84
    * ROBLOX TOYS + FREE CODES!
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=21eU70g0haw
    * GETTING A DUPER BANNED ON PET SIMULATOR \#SavePetSim
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=HJ0cDV1bL1E
    * WE GOT THE BEST PET IN THE GAME! (Broken)| Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=4RiFXXbcRxk
    * I MADE MY HYDRA RAINBOW (Rarest Pet) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=sZN1BaRSu_A
    * I GOT THE RAREST PET (Update 10) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=sP9M-XP7daM
    * TIER 18 (Make ANYTHING Gold) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=RaLzpRA50zg
    * 150k Q&A + ROBLOX TOY CODES GIVEAWAY!
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=wfm7PSJD8x4
    * THE OWNER GAVE ME THIS PET | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=fRHHDS48bUw
    * BROKEN MONEY &GOLD DOMINUS HUGE ACCOUNT GIVEAWAY | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=rTEsPdor8Q8
    * HOW TO DUPE PETS & NOT GET BANNED! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=KqaVxFmkanU
    * I GOT BROKEN MOON COINS WITH THE BEST PET | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=7MjWRcRsiTE
    * EVERY DOMINUS HUGE vs DOMINUS CHEST (FAST)! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=7aGDLJodIbQ
    * NEW JUMP LAUNCH GLITCH | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=NvlabWaEW70
    * NOW I CAN PLAY PET SIM ALL DAY (E-win Chair)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=LCr1HjC6mlI
    * RAINBOW DOMINUS HUGE (Rarest Pet) vs DOMINUS CHEST | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=TJvnatFkhTw
    * I BOUGHT 5 MILLION COINS | Weapon Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=7JOAwm3vZm8
    * 1 DOMINUS HUGE vs. THE DOMINUS CHEST | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=h5Yu4cbqrT8
    * I GOT BROKEN MONEY WITH THE BEST PET | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=WIpeLJn6eHY
    * GOLD AND RAINBOW HATS | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=fZ0BWe9VA00
    * MY NEW BEST PET (Super Good) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=umYGQjZfOP4
    * MY BEST PET VS THE DOMINUS CHEST (oof) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=g6-xCB6fxdQ
    * DESTROYING THE DOMINUS CHEST SOLO (Fast) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=e4NQUq5oDMQ
    * NEW UPDATE (Best One Yet) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=3Umg6QCXCU0
    * NEW GAMEPASS AUTO PILOT (Idea) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=dv5en832qEE
    * So i Played Pet Simulator With Rthro (Anthro)......
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=9tQiFpoplQ0
    * I GAVE HIM THE BEST PET | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=iZVvMw8-sVs
    * MY INFINITE PETS GAMEPASS STILL WORKS! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=PYygObpnydg
    * OPENS LIMITED TIME EGGS & GOT 2 RARE PETS! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=6nJWEAtiPNg
    * BACON HAIR GETS THE RAREST CHEST | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=LtQTpuGB3c0
    * HALLOWEEN UPDATE (Rare Pets) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=q_OzDgMEMj4
    * I GOT HACKED (800,000R$) & ALL MY STUFF | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=yhuRpmowFrw
    * HE GOT ME THE RAREST HAT IN PET SIMULATOR
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ebqoD6gUNLk
    * THE OWNER GAVE ME UNLIMITED MONEY | Timberds Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=EFwIFUr8Vo4
    * HE BOUGHT ME THE INFINITE PETS GAMEPASS 40,000R$ (Trolling) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=xpRmKckMBHQ
    * \*NEW SCAM\* Hat Stat Scam | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=83FGqQRLht0
    * THE WORST PET vs THE BEST CHEST (Shocking) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=4nrjzHEoCGw
    * BUYING THE NEW GAMEPASS (100 Hats) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=OXbpOwrJWhA
    * HATS UPDATE! (Super OP) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=YeYkmn-ZB4o
    * One Plot Challenge Getting Started (Season 2) | Lumber Tycoon 2
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=-iwYu8QAm-c
    * 50 RAINBOW SHOCKS vs RAREST CHEST | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=LyXiheH_zYw
    * WATCH ME DUPE (Trolling) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=CRd2Reni498
    * 50+ KILLS SNIPER GAMEPLAY (Crazy Feeds) | Fray
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Znroxk1_j5Q
    * BUYING THE HEADLESS HORSEMAN FOR MY ALT ACCOUNT (31kR$) | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=4LthwyDvk6o
    * PRONE ONLY CHALLENGE | Fray
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=GlqArI4Gli8
    * GOING INVISIBLE IN FRAY | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Vhsh4i79ARY
    * NOOB vs PRO vs ANNOYING PEOPLE | 💥 Super Power Training Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=s6iUq8b_BlI
    * RAINBOW TIER 16 ONLY CHALLENGE! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=jZT2kJnuacQ
    * BUYING THE RAINBOW CORE SHOCK |Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=5qprpXnex5I
    * FRAY- The Beginning | Montage
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=5w_vt38Fx6I
    * NEW \*EXCLUSIVE\* CODE! (Free Pet) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=fQFDvGriBkI
    * HUNTING FOR RAINBOW CORE SHOCK | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=YoyuFTBz7qs
    * I GOT THE RAREST/BEST PET | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=h4-5aWy5Hmg
    * BUYING ALL THE GAMEPASES (1,000 R$) | Fray
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=NVCg3RMZxoU
    * GETTING MY FIRST SUPER POWER!| 💥 Super Power Training Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=cPanr7dFVQY
    * I PAID 5k ROBUX FOR THIS (anthro) SNIPER | Fray
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=4sdbD0SRnBg
    * HOW MANY COINS WILL WE GET ...... | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=uJBc0NcL1gA
    * HALLOWEEN OUTFIT 2018! | Spooky
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=MDkrpIH0-_8
    * BEST PET IN UPDATE 6? | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=KzoEfbrRBbE
    * CHEST ONLY CHALLENGE | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=pl6ogVSrUGg
    * NEW UPDATE CYBORG PETS & EGGS (Teleport) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=v1BdNLLmsEg
    * DELETING ALL MY RAINBOW MORTUUS | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=lg9BgtaSByU
    * I Tried To Give a NOOB Pets and This Happend... | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=43HfOVrpGIs
    * FINALLY WE GOT RAINBOW DOMORTUUS | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Z3tBGMP-6Ac
    * New Game \*EXCLUSIVE ITEMS\*(Codes) | Lumberjack Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=fqiqWkDc5fs
    * BEST ITEM IN THE GAME! | Dragon Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=3TGszKeHrVw
    * I HOPE I DONT GET BANNED FOR THIS...... | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=OrhSCBAgPvQ
    * CONVERTING COINS TO MOON COINS UPDATE (Soon) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=89GgefcB7eU
    * ONE PET CHALLENGE (Race to Space) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=QucCpS7zdNc
    * WE GOT DOMOURTUS AND RAINBOW AME DAMNEE | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=_Vvo8WtnBsc
    * GIVING RussoPlays THE BEST PET IN THE GAME! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=fB7qHrD39w4
    * She Gave Me The BEST Pet in PET SIMULATOR!! | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=jiNRP3_0LRU
    * HOW TO GET COINS \*MEGA\* FAST | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=aul1ZXoknpg
    * MAKING MY RAREST PET GOLDEN | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=KdKselIwWWE
    * BUYING THE NEW GAMEPASSES (Good or No) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Lt2j6zuSNrs
    * MAKING GHOST TREE A ROBLOX ACCOUNT
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=xNL14F5lWv4
    * FAN HANGOUT (Speed Build) PT.2 | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=DB3uogB1E7k
    * PRO HELPS NOOB | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=bTt_frdCHlE
    * I GOT THE NEW RAREST PET IN THE UPDATE! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=q7VSjrNFKZE
    * FAN HANGOUT (Speed Build) PT.1 | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=absSSx9YkJ8
    * MY SECRET TALENTS | Vlog \#1
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=xBrUokimOpc
    * NEW EXCLUSIVE CODES w/CodePrime8 | Mining Simulator X
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=maogxj6cerQ
    * ANTHRO IS HERE AND I LIKE IT | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=OQPt3gsKXQE
    * GIVING AWAY ALL MY PETS | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=JRsiXPX1cpU
    * PvP IN DESTRUCTION SIMULATOR | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=sVuWA3odChI
    * I MADE HIM THINK THE BEE IS THE RAREST PET  | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=N8fUBu6daPQ
    * I WILL BE THE BEST! | Parkour Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=c3lMU1W7In4
    * HE GAVE ME THE BEST/RAREST PET IN THE GAME | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=tepd7ppa4EU
    * I GAVE A NOOB THE RAREST PET | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=OIFlqMtpVRA
    * PRO HELPS NOOB | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Prx6TVaV6PQ
    * HELPING FANS! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=mP9j0_aNBQU
    * \*EXCLUSIVE\* CODES! | Mining Simulator X
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=CwY8oSSldiM
    * GIVING AWAY ALL MY OLD PETS! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=WqPU2UuyT3Q
    * NEW BEST PET (Updated Mortous) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=S3GlpESeD2M
    * NEW GOLDEN 1NE PET | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=wLaN_gW2J-E
    * FIRST LOOK | Mining Simulator X
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=2VJdVAD-a4M
    * HUGE UPDATE! (TIER 15+CandyLand) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=LLENTMgcByA
    * HOW TO GET A FREE V.I.P SERVER | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=PNTuWRl-_Aw
    * Pet Simulator CHALLENGE (no Robux) | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=q3sUGqZLIvE
    * I FLOODED Yard Work Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=9kLzH_Ht4zw
    * PAYING STRANGERS TO BUILD ME A HOUSE | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=gv4MOZWRU6g
    * NEW! 💥Destruction Simulator Black hole | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=JHcWmecu_ts
    * PRO HELPS NOOB | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=yIUYCMgU3Us
    * I GOT THE 2 RAREST PETS | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=yDlhlAJ3DJQ
    * I EQUIP 500 TIER 14 PETS THEN THIS HAPPEN ....
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=T9Hi6syAjc0
    * THE NEW RAREST PETS IN THE GAME | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=lSdVk_yVfCY
    * BUYING TIER 14 EGGS (Super OP) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=E7u_CF83g6U
    * BUYING ALL \*NEW\* GAMEPASSES | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=VAp5OMzafSs
    * SPACE UPDATE! (Broken) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=HeRVxu_ckFE
    * NEW ZONES,CHESTS & PETS (Leaked) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=WmHKfHRgJBY
    * USING HACKED PETS! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=FOCBfwpiOe8
    * PETS ARE BACK (OP) | Yard Work Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=UvdIxqZGSTA
    * HATS FOR PETS! (Coming soon) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Bzdm4PWbI0Y
    * MAKING SPACE FOR THE \*NEW\* TIER 13 PETS | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Kxu4swFGLaQ
    * THE OWNER SHOWED ME THIS... | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=QwPwtbmpkC0
    * i used 100 tier 12 pets, then this happened... | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=gZ3U0_clNw8
    * NOOB vs PRO vs ROBUX SPENDER | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=z3-Yx2NYeLU
    * BUYING THE INFINITE PET GAMEPASS IN PET SIMULATOR (40k ROBUX) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=l9dfsynl1U0
    * HOW TO GET 2 BILLION COINS WITH 1 CHEST | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=bugUjvH9kq4
    * GOLD PETS , TRADING & EXPENSIVE GAMEPASS | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=GDB7F1AG9wc
    * SHOP SPEED BUILD (New Game) | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=r_EK9-T_CIw
    * HOW TO SPAWN CHEST! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Amm4DSP3VN0
    * THERE IS A 1% CHANCE TO GET THIS EGG | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=M2Vt50cCK8Y
    * JAILBREAK UPDATE (RPG,GRENADES,New Car) | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ZcVDLYqaIVM
    * Free For All MOD TROLLING | Bo2
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=boV9Kv7bazQ
    * BUYING EVERY GAMEPASS (Super OP) | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=U9c3qMT5oqM
    * LINKMON99 SENT ME A TRADE | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=FqaE3Al3bVk
    * CHICKEN SPAWN GLITCH (Helping Noobs) | Egg Farm Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=r_u8G7yPTKA
    * UNLIMITED EGGS FROM THE DEALER | Egg Farm Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=6GO7BSu4YBY
    * Noob vs Hacker vs Pro \[ Island Royale Edition \]
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=15s_8xqMaG4
    * I BOOSTED THE WHOLE ROBLOX SERVER! | Yard Work Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=2-1gyuXljMc
    * I-R-R-E-L-E-V-A-N-T | going to skatepark
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=QhgI-pdmU70
    * BUILD A BOAT OBBY w/ Z_Nac (SUPER FUNNY) | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=y4d0uJrrkas
    * Egg Farm Simulator First Look! | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=7KCeh8nwymQ
    * DOMINUS GIVEAWAY!!! | Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=BzYg6a-y3P4
    * WE MADE A PENGUIN ARMY (Unlimited Money) | Beach Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=XmhbSQNeglY
    * RELEASING ALL MY PETS (New Pet) | Beach Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=aMyi5meSMVQ
    * Mining Simulator REVENGE!
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=5lCMZRc2kXA
    * Bad Roblox SUPER DURP
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=psxDS3Wu8T4
    * Filling The INF. Bag | Yard Work Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=8AeM-NJ-F6c
    * I DevEx 1M R$! |Roblox
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=aXVuFp4d96M
    * FIRST EVER Pets Only Boss KILL! Snow Shoveling Simulator❄️| Roblox
      * Description references the data collection website robux.network.
      * URL: https://www.youtube.com/watch?v=66SKEiRH-W4
* ItsMatrix (MatrixPlaysRB)
  * Information Collection
    * Racing Is Finally Here! \*HUGE UPDATE\* (Roblox Ultimate Driving)
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=4ihekd29k4U
    * The Biggest Chest I've Ever Seen in Pet Simulator.. \*omg\*
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=cC7nJLOO1dc
    * We're Hunting Ghosts in Roblox!! \*EPIC NEW GAME\* (Ghost Hunting Simulator)
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=m1GB_G0oDv8
    * \*OMG\* Custom Bey-Parts in Beyblade Rebirth! \[BEST UPDATE!\]
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=E7f54LayIDI
    * This is The Coolest Simulator of 2018! \[Codes\] (Roblox Ninja Simulator 2)
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=w5xuqq5FPJ8
    * Best Budget Drift Build! (Roblox Vehicle Simulator POOR to RICH Episode 5)
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=Ms_uLpvpEN8
    * I Returned to YouTube! + Drifting Simulator News.
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=YRKwvRsNf7k
    * Claiming HUGE Bounties As A Mercenary in Wanted! \*Roblox GTA5\*
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=OcuPtUNvwWE
    * Best Exotic Super Car Under $1,000,000! (POOR to RICH Episode 4)
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=I2ef-FFW9qk
    * Robbing The \*NEW\* Jewel Store, & Treasury in Roblox GTA 5! (Wanted: New Update)
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=tKnirMp6jJk
  * Non-Giftcard Robux Giveaways
    * FULL THROTTLE RACING TOURNAMENT! \[SUBSCRIBERS WIN FREE ROBUX!\]
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=Oihdfs3SsM0
* Jamie ThatBloxer (Jamiethatgirl)
  * Information Collection
    * HOW TO GET FREE ROBUX AND BC | UPDATED 2016
      * Video is about getting free Robux using an Android app to fill out surveys.
      * URL: https://www.youtube.com/watch?v=eNdg3A5tamY
* JeffBlox (JeeffBlox)
  * Information Collection
    * ISSO FOI REMOVIDO DO ROBLOX E VOCÊ NEM PERCEBEU
      * Description references the data collection website rblx.pro.
      * URL: https://www.youtube.com/watch?v=wAeBdOec_lE
    * ITENS GRÁTIS NO ROBLOX
      * Description references the data collection website rblx.pro.
      * URL: https://www.youtube.com/watch?v=fX6XKPfSrIc
    * DUAS NOVAS COISAS DA ATUALIZAÇÃO DO MAGNET SIMULATOR QUE VAI TE DEIXAR MUITO RICO
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=ZpMSokgl8_w
    * TRANSFORMANDO O MELHOR PET EM BRILHANTE NO MAGNET SIMULATOR(pet do batman)
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=SUBorhYFelQ
    * COMPRANDO O MELHOR IMÃ DO MAGNET SIMULATOR(imã de 6,000,000,000 de tokens rebirth)
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=-saTihFSewo
    * NOOB COM OS MELHORES PETS DESBLOQUEIA TUDO NO MAGNET SIMULATOR
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=GCCDNgGBn3I
    * ABRI VÁRIOS OVOS MÍSTICOS E GANHEI O MELHOR PET NO MAGNET SIMULATOR - ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=ucjA4UN0AJs
    * VOCÊ PRECISA VER O PREÇO DO NOVO OVO DO MAGNET SIMULATOR NO SERVER VIP!!
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=o6Fkv9_WY0A
    * DOMINUS PET GRÁTIS NO BUBBLE GUM SIMULATOR😱(UPDATE 9)
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=BH4ZtcqbdH8
    * ESSE E O MELHOR IMÃ DO MAGNET SIMULATOR(Muito Apelão Parece H4ck)
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=dfWRapQur08
    * \*CÓDIGOS SECRETOS\* DO JETPACK SIMULATOR - ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=B77hPXvqX-0
    * 3 JOGOS FAMOSOS QUE OS SCOOBIS JÁ FAZEM PARTE
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=-Go9l65Vjw4
    * OS SCOOBIS VÃO DOMINAR O ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=rCDdws35KrI
    * NOVO OVO! COMPRANDO 5 OVOS MÍSTICOS NO MAGNET SIMULATOR - ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=81VBJxWV9zM
    * A FORÇA DO IMÃ DO ESPAÇO - MAGNET SIMULATOR ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=lpixDxpkpNY
    * FREE FIRE NO ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=tWJ7HkBbdgU
    * MEU NOVO AVATAR DE ANO NOVO NO ROBLOX 2019
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=1PMAZ6uzAW8
    * MINHA PRIMEIRA VEZ NO CAOS CIBERNÉTICO - BRAWL STARS(com o max de recompensas)
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=4Z7PZjcUfMY
    * NOVO JOGO DE ANO NOVO NO ROBLOX - MAGNET SIMULATOR⭐
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=HJ8oauRhr3E
    * ABRI 54 CAIXAS E GANHEI 2 BRAWLERS ÉPICOS - BRAWL STARS
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=MoJnd4oKczw
    * COMO TER NOME COLORIDO NO BRAWL STARS
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=QI5Y4F674bU
    * DESBLOQUEEI UM BRAWLER ÉPICO NO BRAWL STARS
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=tetlbfuxMso
    * DESBLOQUEEI A ÚLTIMA ÁREA DO PET SIMULATOR - ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=Ji_gQAErBXo
    * COMPRANDO AS SKINS DE NATAL NO BRAWL STARS🎄
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=9GZSJqFKVNY
    * COLETEI UM MEGA TRENÓ E FIQUEI RICO NO PET SIMULATOR
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=C4jcRnHatPE
    * COMPRANDO O MELHOR OVO DA NOVA ATUALIZAÇÃO DO PET SIMULATOR - ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=oqNJYizeD_Y
    * COMPRANDO O NOVO PACOTE DE FIM DE ANO NO BRAWL STARS
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=_t38u9ihmqo
    * ABRINDO VÁRIOS DO MELHOR OVO DE NATAL DO BUBBLE GUM SIMULATOR - ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=maVIyhhktmY
    * NOVO CLUBE DA FAMÍLIABLOX NO BRAWL STARS
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=tg0eztrtiHc
    * VICIEI NESSE NOVO JOGO - BRAWL STARS
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=WjQPIMH4UN0
    * TROCANDO MUITAS GEMAS POR MUITOS CANDYCANES NO BUBBLE GUM SIMULATOR - ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=Y2xvyVvqUjk
    * ☃️CÓDIGO SECRETO DO SNOWMAN SIMULATOR - ROBLOX☃️
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=Vy6ptZM8Xmo
    * SUPER VELOCIDADE NO JAILBREAK ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=KkMdHL3Kxfo
    * COMPREI A NOVA ILHA E A MELHOR FERRAMENTA NO TREASURE HUNT SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=d_Rf-rjcepw
    * COMO ENCONTRAR MUITOS BAÚS E GANHAR MUITO DINHEIRO NO TREASURE HUNT SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=rfZC04MZkNE
    * COMO IRRITAR UM JOGADOR DE ROBLOX 😄
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=cUzXIX74Ngc
    * O ROBLOX JÁ ESTA ADICIONANDO O NOVO R62?
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=hkYUKBjqBqY
    * NOVOS CÓDIGOS!! COMPREI A MELHOR FERRAMENTA E FIQUEI MUITO RICO NO TREASURE HUNT SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=S1A0B-4kh2o
    * NOVO CÓDIGO! E COMO ACHAR E MATAR O NOVO BOSS NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=-hayS7t6D0o
    * SOU O BRASILEIRO MAIS RICO DO CASH GRAB SIMULATOR?? - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=DfFL2hsTrVI
    * RELEMBRANDO OS VELHOS TEMPOS NO PRISON LIFE - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=hgYxGJSJySY
    * OS CAÇADORES DE HACKER NO JAILBREAK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=2YzP-UI4FV8
    * NOVO MINIGAMES BRASILEIROS NO ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=CScn3ENIg9I
    * PEGUEI 22 ADMIN E FIQUEI RICO NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=FJiJ2znKipM
    * OS BANDIDOS MAIS ATRAPALHADOS DO JAILBREAK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Is9UYjKUKfU
    * NOVO CÓDIGO E NOVA FORMA DE DOAR DINHEIRO NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=QZAw9y-UrK8
    * USEI A NOVA FERRAMENTA E PEGUEI 4 CLIENTE ADMIN E 16 GOD NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=fXnf825pmkg
    * PALHAÇO ASSASSINO NO JAILBREAK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=N9tm0S0ZeSU
    * COMO ACHAR E PEGAR O CLIENTE ADMIN NO CASH GRAB SIMULATOR - ROBLOX(FT.BIELHENRIIK)
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=m70wtcdISUo
    * PEGUEI UM CLIENTE ADMIN E TRANSFORMEI TODOS EM OURO NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=GQLIgxlizXs
    * BOLA DE GELO MISTERIOSA!! DA DINHEIRO?? SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=L9EqSqpTqWQ
    * SOU UM CACHORRO POLICIAL NO JAILBREAK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=OsanGDGMpso
    * COMPRANDO OS NOVOS PETS NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=AHfsX9Nckos
    * OS 5 MELHORES JOGOS BRASILEIROS NO ROBLOX 🎮
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=qpXDK_uNJCw
    * COMPRANDO A MELHOR FERRAMENTA E CAPTURANDO O CLIENTE ADMIN NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ss6u6Y20rq8
    * COMPRANDO A NOVA ARMA PRA MATAR O ICE BOSS NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=vxHVUhEAwZo
    * COMPRANDO O VIP E GANHANDO MUITO DINHEIRO NO JAILBREAK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=3I4_-pGoKcw
    * NO ESTILO GTA COM O BUG DA ARMA NO JAILBREAK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=IcraNErKAhQ
    * O NOVO CLIENTE QUE DA 5 MILHÕES DE DINHEIRO NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=xVaYE5Xcbiw
    * COMPRANDO A MOCHILA QUE CUSTA 1 MILHÃO E FICANDO RICO NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=2w8Rmp23Pvk
    * NOVOS CÓDIGOS PARA GANHAR DINHEIRO E SPEED NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=O_j0XhOTd6U
    * VENDENDO MUITOS JOGOS LEGEND E FICANDO RICO NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=z05OXn0ZZt0
    * COMPRANDO O BANK VAULT E FICANDO RICO NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=71wyB9bQnL4
    * NOVOS CÓDIGOS!! E PULANDO AS RAMPAS DA MONTANHA NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=9kf0gBkWoXI
    * PRENDI MEU IRMÃO NOS TRILHOS DO TREM NO JAILBREAK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Y--yRnySak0
    * NOVO CÓDIGO!! E MATANDO O ICE BOSS SOZINHO NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=cPkfv0IeOR0
    * CÓDIGOS PARA GANHAR DINHEIRO E SPEED NO CASH GRAB SIMULATOR - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=sBQ7G0H2Z3s
    * O POLICIAL SUPER SAYAJIN NO JAILBREAK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Hz0hHU0gULk
    * NOVO CÓDIGO!! E COMPRANDO O ICE HAMMER NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=LFB68u9UFDA
    * NOVA ATUALIZAÇÃO DA MONTANHA NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Q7jeovxl0mQ
    * JOGANDO BLOXBURG PELA PRIMEIRA VEZ NO ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=RD9wR7EHoHI
    * NOVOS CÓDIGOS DE MONEY E EXPLORANDO A MONTANHA NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=iKCC78zIb_0
    * COMO FUNCIONA O TELETRANSPORTE NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=2Ti4kZQqca0
    * COMO PEGAR E VENDER O CUBO DE GELO(ICE CUBE) NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Gx8QziDDL3k
    * COMPREI O PET QUE CUSTA 1000 ROBUX NO ZOMBIE ATTACK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=iTTYemoOw8c
    * ENTREGUEI 20K DE ICE AO CAVE EXPERT E ACHEI ICE RARO NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=LgDc5EVewOw
    * COMO ACHAR E MATAR O ICE BOSS NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=MCOVo8iuy5E
    * COMPRANDO O NOVO PET NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=v7sdzWjXgEw
    * COMO PEGAR O NOVO ICE E VENDER NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=yzWyRXde5ME
    * OS POLICIAIS VÃO TER UM CACHORRO NO JAILBREAK? - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=0OlNj3P7Lrk
    * COMPRANDO O NOVO PET O DRAGON  NO ZOMBIE ATTACK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=L5HYmkpL1FI
    * COMPREI O NOVO SNOWMOBILE E ENTREI NA ICE MOUNTAIN NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ZMS8IJ_zR8I
    * A MALUCA MOTO ABOMINÁVEL DA NEVE NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=pQB4HlCtzVw
    * VOANDO DE HELICÓPTERO POR BAIXO DO MAPA DO JAILBREAK - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=JthF0OknG4M
    * O CRIADOR DO SNOW SHOVELING SIMULATOR FEZ UM NOVO JOGO NO ROBLOX!!
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=S1NBe05glJE
    * DUELO DE YOUTUBERS NO ROBLOX - JEFFBLOX VS EMPIREBLOX  (CAPTURE THE FLAG)
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=VIXC3IqkDB8
    * DESAFIO VOCÊS A ESCAPAREM DE MIN NO ROBLOX!! (ESCAPE DO JEFFBLOX)
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=_bE4W0RRdzU
    * COMO COLOCAR SUA MÚSICA PREFERIDA NO ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=l1jE4GBk5CE
    * PEGUEI LEVEL 1600 NO ZOMBIE ATTACK - SAIBA COMO EU PEGUEI TÃO RÁPIDO(ROBLOX)
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ztzqb-sZp-I
    * PEGUEI 5 MILHÕES DE MONEY NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=5UeaEbSEMYg
    * O JOGO DOS YOUTUBERS BRASILEIROS NO ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=pj44GwNnF7k
    * COMO GANHAR ROBUX GRÁTIS COM O SEU JOGO DO ROBLOX
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=3KAhMImyAY0
    * JOGANDO A CÓPIA PERFEITA DO ROBLOX 😲
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=EMezvUp94No
    * FIZERAM UM JOGO DE PERGUNTAS SOBRE MIM NO ROBLOX(QUIZ DO JEFFBLOX)
      * Description references a video to a Robux giveaway
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=WJ6jkPYWKPs
    * IMPOSSÍVEL JOGAR ESSE JOGO DO ROBLOX SEM ROBUX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=-xADiLtaaXA
    * SUPER MODO PALHAÇO CRIMINAL NO JAILBREAK ROBLOX!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=07h0tj5pBI0
    * ICE BOSS NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=hEFIRzhCQq4
    * PEGUEI LEVEL 600 E DESBLOQUEEI MAIS ESPADAS E ARMAS NO ZOMBIE ATTACK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=8NUy94o7UQg
    * COMPRANDO A DIAMOND BOMB E TRANSFORMANDO NEVE EM DIAMANTE NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=VsAptUEiWK4
    * COMPRANDO O NOVO PET E FAZENDO BURACOS NEGROS NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=au-3SdBtJg0
    * NOVO CÓDIGO DE PET E COMPRANDO O PET DE 500,000 MONEY NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=9y35Lqjafco
    * OQUE VAI VIR NA ATUALIZAÇÃO DE EXPANSÃO NO SNOW SHOVELING SIMULATOR ?❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=B7AHcVZc4ms
    * PASSEI DA WAVE 50 NO NOVO JOGO DE ZOMBIE NO ROBLOX(Zombie Attack)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=bJrKngGb9O0
    * COMPREI TODOS OS VEÍCULOS E GAME PASS DO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=aNlx3ZvpjaU
    * OLHA SÓ OQUE ESSA MARRETA FAZ NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ubV4FUJZKbg
    * NOVO CÓDIGO DA MOCHILA TV E SUPER ATUALIZAÇÃO NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=-x9vxb9bkgk
    * COMPRANDO O GRADER E FICANDO RICO NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=I4Z3WL2ugSw
    * TENTANDO ARRANCAR O TREM DOS TRILHOS NO JAILBREAK ☃️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=qm2MZrbx_ys
    * COMO TER UMA ROUPA IGUAL A ESSA PACKAGE DE GRAÇA OU POR 10 ROBUX NO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=WNUenod7pGk
    * NOVOS CÓDIGOS PARA GANHAR BUCKS GRÁTIS NO ADOPT ME - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=nW-HsrjUD7g
    * COMPRANDO A NOVA MOTO ABOMINÁVEL NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=vCNCDoot2CU
    * ESSE JOGO DO ROBLOX DA MUITO SONO - SLEEPING SIMULATOR(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Jv8tkJkIFR8
    * SUPER ATUALIZAÇÃO NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=qoxMKLuuWS4
    * COMBINE ESSES DOIS E FIQUE RICO NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=cWk8tnHZEZc
    * COMPREI O THERMAL SUIT E DERRETI NEVE COM MEU CORPO NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=-u4RLZcBdhs
    * ESSE JOGO DO ROBLOX ME ENGANOU COM TROLLAGENS
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=qlz8MnE_m58
    * TODO MUNDO PEGANDO MUITA NEVE NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=u1YyD9UCpyo
    * COMPRANDO O TRENÓ E FAZENDO MUITO DINHEIRO NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=zmZy3lI_VYs
    * COMPRANDO O LIGHTNING BOLT E FIZ MUITO DINHEIRO NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=LDF9kyAgA-s
    * GUEST VS NOOB NO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=iJWudyspNng
    * PEGANDO NEVE INFINITA NO SNOW SHOVELING SIMULATOR ❄️ - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=lkDq03todcw
    * O FUTURO DO ROBLOX 😧
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ee6tPWu6KHY
    * JOGANDO O JOGO QUE CUSTA 1000 ROBUX NO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=o0fbdeBXI18
    * ROBLOX REAL VS ROBLOX CÓPIA
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=o4HXxhVDCxM
    * DESCOBRINDO COISAS SOBRE O ASIMO3089 CRIADOR DO JAILBREAK \*2018\*? - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=1FPmTcQMdfI
    * O TYCOON DAS REDES SOCIAIS NO ROBLOX - SOCIAL MEDIA TYCOON(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=VkavPubxhtc
    * VALE A PENA COMPRAR BUILDERS CLUB NO ROBLOX ?
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=buxEaP7RU9E
    * TROLAGEM!! KIKANDO PESSOAS DO SERVER NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=wV89OcqRQNQ
    * OS 10 ITEMS CARO DO ROBLOX QUE ERAM QUASE DE GRAÇA!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=4wNTbIv1VnI
    * ITEM TIX ESCONDIDO NO ROBLOX :O
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=olaK1NvRyOg
    * O JOGO MAIS CARO DO ROBLOX 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=NUmSdOxBu6I
    * COMO COLOCAR O TEMA DO ROBLOX EM SEU NAVEGADOR (TEMAS GRATUITOS PERSONALIZADOS)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ocqnWjkK_Iw
    * OQUE VOCÊ PREFERE ? NO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=zPAQxJB8agE
    * FIZERAM UM JOGO SPEED RUN PARA MIN NO ROBLOX!!(MODO HARD)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=KEPHatcs_tg
    * ENTÃO, VOCÊ ACHA QUE CONHECE O ROBLOX??PROVE!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=WtaI4saHPDo
    * O JOGO DE ENCONTRO HACKERS NO ROBLOX!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=DHqpbwkCvjo
    * VOCÊ E UM NOOB OU UM PRO NO ROBLOX??DESCUBRA!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=TMysDoFKfDo
    * O JOGO MAIS ODIADO DO ROBLOX 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=n-KeSAr0gWg
    * NOVOS SEGREDOS DO JAILBREAK SERÃO REVELADOS :O (ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=XXUPI8R-ymU
    * CÓDIGOS PARA GANHAR DINHEIRO GRÁTIS NO SNOW SHOVELING SIMULATOR
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=eUzE5LR_sM8
    * FINALMENTE O TREM NO JAILBREAK E NOVA FORMA DE ROUBO - ROBLOX (SUPER ATUALIZAÇÃO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=A2iX-V9wTXU
    * VOCÊ E UM VERDADEIRO ROBLOXIANO?? DESCUBRA!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=IA4Hc8VlRDI
    * VOCÊ TEM MENOS DE 24 HORAS PARA PEGAR ESSE NOVO ITEM GRÁTIS DO ROBLOX :O
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=X2X3JBPp04A
    * MEU VÍDEO ESTA NO \#48 EM ALTA OBRIGADO A TODOS :D
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=v-xii6jBEYg
    * O ÚLTIMO GUEST O JOGO NO ROBLOX 🎮
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=z6PEPEvv2Dc
    * SAIUUU!! SEGUNDO PRESENTE GRÁTIS COM ITEM MISTERIOSO NO ROBLOX!🎁
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=1JL-W_fBpxs
    * ESSE YOUTUBER TEM A CONTA MAIS TOP DO ROBLOX!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=g6YUA4ohLFs
    * 10 COISAS QUE VOCÊ NUNCA SOUBE SOBRE O ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=G-1Olg4XJb4
    * O DIA EM QUE O ROBUX FOI ADICIONADO NO ROBLOX💸
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=34WI-ffIsFU
    * ALGUÉM AINDA LEMBRA DESSE JOGO DO ROBLOX??
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Jeu5xtPghNk
    * NOVO VEÍCULO 1M NO JAILBREAK - ROBLOX(SUPER ATUALIZAÇÃO DE INVERNO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ODzhptZnc54
    * O JOGO MAIS MALUCO DO ROBLOX!!(Gravity Shift)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Xnqa73WdD7U
    * SAIUUU!! NOVO PRESENTE GRÁTIS COM ITEM MISTERIOSO NO ROBLOX!🎁
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=sqNAnPKpSxI
    * TODOS OS COMANDOS DE MOVIMENTOS DO ROBLOX(Animation System)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=1Xplio6NoY4
    * O ROBLOX VAI NOS DAR PRESENTES COM ITENS??🎁
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=JQHrW7G3c2k
    * OS JOGOS MAIS RICOS DO ROBLOX 🤑
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=KPz-V7vlqJA
    * SUPER MODO ZOMBIE NO JAILBREAK ROBLOX!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=SbYk4zw3iI0
    * TROLANDO GRINGOS COM ÁUDIO DE ALERTA HACKER NO JAILBREAK??
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=gvz7tdHGKL0
    * HACKER VS ADMIN NO ROBLOX!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=31t5ByRJ6xg
    * NOVO CÓDIGO QUE DA 100MILHÕES DE CASH NO ZOO TYCOON - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Zh52WvCNECo
    * O ANTHRO R30 JÁ FOI ADICIONADO EM ALGUNS JOGOS DO ROBLOX!! 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=bNG0WOfWA9Q
    * 5 COISAS QUE PODERIAM SER ADICIONADAS NO ROBLOX!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=RYUJJWEsMAE
    * FÓRUM REMOVIDO DO ROBLOX (R.I.P FÓRUM)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=A29nYcd0RCA
    * COMO SERIA O ROBLOX NO ANO DE 1950 VOCÊ NÃO JOGARIA O ROBLOX ASSIM!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=CKBXEgywprk
    * O CATALOG DO ROBLOX EM 2007 🤑 (10 ANOS ATRÁS)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=H0sBEvq-SEg
    * COMO ATIRAR DEITADO NO JAILBREAK ROBLOX (NOVO GLITCH)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=SekKYZ9JwiA
    * ✔COMO FAZER UM NECK(ITEM DE PESCOÇO) DE GRAÇA NO ROBLOX‼
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=eM100E9h9aM
    * AS 5 CAMISAS MAIS PERSONALIZADAS DO ROBLOX - ISSO E REAL??😲!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=nUReinqRF9A
    * O TABLET QUE CUSTA 1 BILHÃO DE ROBUX NO ROBLOX!! EU TENHO ELE??
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=A0fwZunJcjo
    * VISITANDO JOGOS ABANDONADOS NO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=CMC3hCtkCQw
    * 3 NOVOS VEÍCULOS NO JAILBREAK E MUITO MAIS - SUPER ATUALIZAÇÃO(ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=7dOC2HchtCg
    * COMO MUDAR O TEMA DO ROBLOX (TEMAS GRATUITOS PERSONALIZADOS)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=qOk-iJN7s9w
    * ROBLOX FALSO VS ROBLOX REAL
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=-k_fMtbeFqM
    * ROBLOX - SOU UM CAVALO E MONTARAM EM MIN NO HORSE WORLD - FINALLY(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=CP1jmCwb2-Q
    * IMPOSSÍVEL ACERTAR A SENHA DESSA MANSÃO - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Tsq1mMby-NI
    * ESSE CARA GASTOU MAIS DE 30 MILHÕES EM APENAS UM ITEM DO ROBLOX!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=VUDUz51fWME
    * SOU O HACKER BOB ESPONJA E O PATRICK NO ROBLOX!! :D
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=f7rPGkw79D0
    * FUJA DO GODENOT NO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=yNrIUUgBCwM
    * SIMULADOR DE PAPAI NOEL NO ROBLOX - SANTA SIMULATOR(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=8gMFZ5noSdY
    * UMA HISTÓRIA DE TERROR COM O MEU NOME ?😲 - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=DWSebSwU_mU
    * A XJ6 MALUCA NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=i8m1w342IpU
    * ESSE JOGO VAI ULTRAPASSAR O JAILBREAK ? 😲- ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=q0Y0l38XcIY
    * NOVO CARRO TESLA ROADSTER 2020 EM BREVE NO JAILBREAK - ROBLOX(MELHOR QUE A BUGATTI??)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=6PsJoNIQwvc
    * COMO TER UMA ESPADA NO JAILBREAK OFICIAL - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=cK-YQVqhqow
    * COMO FAZER A CAMISA DO FELIPE NETO "REBULIÇO" NO ROBLOX DE GRAÇA!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=dVsKOSwK7mc
    * ANTHRO R:30 ESTA SENDO ADICIONADO NO ROBLOX? 😲(KEN E BARB TEST)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=mbwaWrHo74g
    * COMO FAZER A CAMISA DOS "IRMÃOS NETO" NO ROBLOX DE GRAÇA!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=yLcRrQpTFxI
    * CUPHEAD NO ROBLOX ?  😲 (CUPHEAD IN ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=LU4r70fZphY
    * ESSE CARA TEM TODOS OS ITEMS DO ROBLOX!! 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=jJ3q_8a_38A
    * COMO FAZER UMA CÓPIA DO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=sWgPECUW6_Q
    * ANDANDO DE HOVERBOARD NO JAILBREAK ROBLOX ? 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=1vlUxB2HyDs
    * OS INSCRITOS ME DERAM UMA LAMBORGHINI NO JAILBREAK ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=R5jKA3rRX5w
    * SEJA UM YOUTUBER BRASILEIRO NO YOUTUBER BR TYCOON - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=jYDlv2yl2XQ
    * TODO MUNDO USANDO ADMIN COMMANDS NA COPIA DO JAILBREAK ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=-IDbR_JPd5w
    * VAI TER CLIMA DE INVERNO NO JAILBREAK ROBLOX ??(ATUALIZAÇÃO EM BREVE)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=XZF4x1JF1JU
    * 5 ROUPAS QUE ME FAZEM SER UM CAMALEÃO NO JAILBREAK ROBLOX (invisível ?)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=QvQaCsYMHTk
    * ESCAPE DOS YOUTUBERS  BRASILEIROS NO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=cwLnbVSgbgw
    * FOMOS PARA SATURNO DE FOGUETE NO JAILBREAK ROBLOX ? 😲(FT.KAPOLAR)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=kq3EsSfM760
    * TREM E FOGUETE SECRETO NO JOGO CRIADO PELO DONO DO JAILBREAK
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=2CEK9-dfRbw
    * GASTEI MAIS DE 5MIL ROBUX NESSE JOGO E DEPOIS PAREI DE JOGAR  😲(ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=XHDfiNzmSkY
    * ACHEI ALGO SECRETO NO KNIFE SIMULATOR ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=AlzPbXL0da8
    * SIMULANDO UM TORNADO NO JAILBREAK ROBLOX DESTRUIU TUDO ? ?
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ZgY1eEu1zes
    * FOMOS PARA LUA DE FOGUETE NO JAILBREAK ROBLOX ?😲(FT.KAPOLAR)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=gvWAXUQCmVE
    * ROBLOX - O SIMULADOR DE FACAS - KNIFE SIMULATOR(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=0KSq-OCcvk8
    * OUTRO JOGO CRIADO PELO CRIADOR DO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=k45icvCePsM
    * A IDEIA DO TREM COMEÇOU HÁ 3 ANOS ATRAS (JAILBREAK ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=wU_9p3BJlF4
    * ESSA ROUPA ME DA MUITA VELOCIDADE NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=cz-jK3ijDjs
    * PINGUIM GLITCH VS SNOWMAN GLITCH NO JAILBREAK ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=yqMphvA2rDw
    * ESTE JOGO SUBSTITUIRÁ O JAILBREAK ROBLOX ? 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=R6ABMIkA4x4
    * COMO TER SEU PRÓPRIO JOGO OBBY NO ROBLOX(TUTORIAL MÉDIO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Vtclg_QqpN8
    * O DOMINUS PROIBIDO DO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=k4YNwWCBj10
    * ROBLOX - CHUTE E PISE NOS OUTROS JOGADORES NO CRUSHING SIMULATOR(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=3Dd8t6vYDiI
    * MODO HARDCORE NO JAILBREAK ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=E92onxTo6kE
    * COMO TER SEU PRÓPRIO JOGO TYCOON NO ROBLOX(TUTORIAL BÁSICO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=vt4m620QeSI
    * ROBLOX - SEJA UM ANIMAL E FUJA PARA NÃO SER CAPTURADO NO PET ESCAPE(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=XMRm9DeI-uc
    * NOVO TRUQUE PARA FICAR INVISÍVEL NO JAILBREAK - ROBLOX(MOTORISTA FANTASMA)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=EkAJSD0YWNc
    * ESSA CAMISA TE DEIXA INVISÍVEL NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=SzK8XoneZdk
    * PRISON LIFE VS JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=RtaU4TDFIDU
    * EMERALD STRIKE A ESPADA MAIS FORTE DO SWORDBURST 2 - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=T9OVcuXO03Y
    * ROBLOX - QUEM TEM MAIS ROBUX E O MAIS FORTE NO ROBUX SIMULATOR(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=iTZmeN1mO3A
    * O CAVALEIRO DE GELO POLICIAL NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=r_EghH7i7ow
    * OQUE ACONTECERIA SE O JAILBREAK FOSSE INUNDADO? (ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=SJrvl82ZJUs
    * OS 30 ITENS PROIBIDOS DO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=UodSMP5LRZo
    * COMO SABER AONDE PASSAR DE NÍVEL RÁPIDO E DROPAR OS MELHORES ITENS NO SWORDBURST 2 - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=WOLJzzKJcHY
    * A VOLTA DO PODEROSO PINGUIM CRIMINAL NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=tMk4IGIUJeE
    * O VERDADEIRO PODER DA SWAT NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=POf6bVSK1JM
    * ESSE CARA CONSEGUIO 23MIL DE BOUNTY | MELHOR CRIMINOSO DO JAILBREAK ?
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=l5PRUWSDEjI
    * PEGUEI LEVEL 36 E FORTALECI MEU EQUIPAMENTO NO SWORDBURST 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=xd9OAjwcAK4
    * O JOGO MAIS VICIANTE DO ROBLOX | SWORDBURST 2(COMEÇA A NOVA AVENTURA)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=BrvLwbAjB1c
    * COMO SERIA SE O JAILBREAK ADICIONA-SE UM HUMANO ?
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=usSJwQ5NE5s
    * QUEM FAZ MAIS DINHEIRO NO JAILBREAK EM 4 MIN | POLICIAL OU CRIMINOSO ??
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=-OUkGzuzZpM
    * ROBLOX - SOU UMA GALINHA COMILONA NO CHICKEN SIMULATOR
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=5jAv3UZeuoQ
    * PRIMEIRO VÍDEO DO ROBLOX NO YOUTUBE E O PRIMEIRO JOGO A SER LANÇADO
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=TykqH1A-m04
    * JOGANDO O JAILBREAK DE OUTRA DIMENSÃO | ENCONTREI ALIENS !! 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=CBdFmTgjdSI
    * CRIADORES DE JAILBREAK MILIONÁRIOS GRAÇAS AO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=tEI7CkQp3CE
    * JOGANDO JAILBREAK COM OS INSCRITOS (ESPECIAL DE 20K)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=tM3FggupXcc
    * NOOB VS PRO NO JAILBREAK ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=0utNkSWfFYM
    * AVISO IMPORTANTE!! SEGUNDO CANAL CRIADO - MUITAS NOVIDADES VEM POR AI !!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=KVQNgC1Ajho
    * ROBLOX - NOVA FUNÇÃO DE ALTERAR SPEED E SERVIDOR VIP GRATUITO - NINJA ASSASSIN
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ksNuuS9K7Bo
    * COMO RECEBER UM AVISO QUANDO UM ITEM NOVO E LANÇADO NO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=JRZc7ATiDZw
    * CONFIRMADO NAVE ESPACIAL NO JAILBREAK - ROBLOX ? 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=cZoGCe9SrgU
    * ROBLOX - TREINE PARA MUDAR DE HERÓI/VILÃO NO SUPER SIMULATOR 2(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=3x2yL9guJLw
    * ROBLOX - VINICIUSDOBR E O BR MAIS FORTE DO NINJA ASSASSIN? 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=dsr95VI-ehk
    * DICAS DE COMO FICAR FORTE NO NINJA ASSASSIN - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=B29uPdK_blY
    * A CÓPIA DO JAILBREAK QUE NÃO DEU MUITO CERTO(ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=bGgAWv8GtUo
    * NOVO JOGO QUE ESTA BOMBANDO NO ROBLOX - CRIE SEU PRÓPRIO MUNDO NO GALAXY SIMULATOR
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=lz8dqqkNUvA
    * 5 COISAS QUE PODERIAM SER ADICIONADAS NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=y42Z-Mvb2Ho
    * COMO TESTAR OS CARROS DO VEHICLE  SIMULATOR ANTES DE COMPRAR - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=XxrZl0DFNeo
    * CRIMINOSO DE SWAT NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=eYUSljERXLY
    * ESCONDERIJO QUE ESTAVA NA SUA FRENTE E VOCÊ NÃO SABIA - JAILBREAK(ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=em-egLQLEIc
    * ROBLOX - ACHEI UM BR QUE TEM QUASE 3 MILHÕES DE FORÇA NO NINJA ASSASSIN
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=pm8XgsvXS08
    * 4 GLITCHES NOVOS NO JAILBREAK | POLICIAL AJUDANTE CRIMINAL
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=LxO5cGye10A
    * ROBLOX - USANDO O CONJUNTO RAINBOW PACK NO NINJA ASSASSIN(CONJUNTO ARCO-IRIS)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=W9kEwp6YJQI
    * COMO PEGAR O HELICÓPTERO SEM PRECISAR DO KEYCARD NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ikL08ORGHdM
    * O JAILBREAK ESCONDE MUITOS SEGREDOS | O MAPA VAI FICAR MAIOR?? 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=BSgjo0A1OYc
    * ROBLOX - CHUVA DE PODER NO TITAN SIMULATOR(NOVA ATUALIZAÇÃO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=musTuMVcy6w
    * CRIMINOSO VESTIDO DE POLICIAL NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=x403vtOR35k
    * COMO TESTAR OS CARROS DO JAILBREAK ANTES DE COMPRAR - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=0HSfw7fAE60
    * ROBLOX - TEM UM BONECO ESCONDIDO NO NINJA ASSASSIN?? (GRANDE MISTÉRIO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=zBB0rCNXeJU
    * O TREM ESTA VINDO PARA O JAILBREAK 😲 ?? - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=C5QR9vzsAyk
    * ROBLOX - TREINE PARA FICAR GRANDE E FORTE - TITAN SIMULATOR(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=D-TjHXynvuQ
    * O TREM JÁ ESTA COMPLETO NO JAILBREAK!!(ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=QHhtsSIvh28
    * TODOS OS LUGARES SECRETOS DO NINJA ASSASSIN - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=tvNunZLt33g
    * ROBLOX - O MAIS VELOZ E O MAIS FORTE - RUNNING SIMULATOR | RACING(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=GdPHbqYmSe4
    * OQUE HÁ DE NOVO NA NOVA ATUALIZAÇÃO DO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=IedD4e1Qy_4
    * OQUE VOCÊ SABE SOBRE O ROBLOX ??
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=owM8K6a6VOU
    * FAZENDO DRIFT E MANOBRAS COM O DUNE BUGGY NO JAILBREAK -- ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=0OGncwHJLY8
    * NOVA ATUALIZAÇÃO DO JAILBREAK PRESTES A LANÇAR(VAI TER CHUVA,RAIOS E MUITO +) 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=4YY4oRwB1eI
    * OS JOGOS MAIS ODIADOS DO ROBLOX ? 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ptRtTMVENY0
    * DUNE BUGGY O CARRO QUE SOBE QUALQUER MONTANHA DO JAILBREAK - ROBLOX(NOVO GLICHT)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=sPDtBX2hP1A
    * NOVA ATUALIZAÇÃO NO JAILBREAK ESSA SEMANA!!(Árvores,Céus,Luzes,Carro e MUITO MAIS!)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=PE9gL8z1oGw
    * JOGANDO JAILBREAK 2 BETA NO ROBLOX ? 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=bnarS9dJ9sw
    * ROBLOX - PULANDO E TREINANDO AO MESMO TEMPO NO NINJA ASSASSIN
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=yKa88Y-f4BE
    * ROBLOX - FAZENDO GRANDES DESAFIOS NO NINJA ASSASSIN(GINCANA?)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=4wzP7Xa7Zdg
    * ROBLOX - SOU O CRIMINOSO FANTASMA NO JAILBREAK
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Te7LyXG4Mwc
    * ROBLOX - SOU O MAIS FORTE DO SERVIDOR - NINJA ASSASSIN
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=R0s-ckSC6Qg
    * COMO AUMENTAR O NINJÚTSU MUITO RÁPIDO NO NINJA ASSASSIN(ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=UMQvXb2CV3c
    * ROUBANDO O MONSTER TRUCK(CARRO DE 1 MILHÃO) NO JAILBREAK ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=VcgeasKv0oI
    * ROBLOX - SOU O POLICIAL FANTASMA NO JAILBREAK
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=WurPjs2h3Ls
    * COMO GANHAR MUITO DINHEIRO NO JAILBREAK - ROBLOX(NÃO E SERVER VIP)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=5_s1mUof3Pw
    * ROBLOX - MAIOR PVP DA MINHA VIDA NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=k1RhMyU2g9Y
    * ROBLOX - PEGUEI 44K DE NINJUTSU E DESBLOQUIEI MUITA COISA NO NINJA ASSASSIN
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=w0fn7r31qsk
    * ROBLOX - ESSE JOGO E MARAVILHOSO - NINJA ASSASSIN (NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=xN_FjXfZ7u0
    * CURIOSIDADES DO JAILBREAK | OQUE TEM DENTRO DO FINAL DO TUNEL DO TREM ? 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Z2sh65Bfq7w
    * REVELANDO LUGARES SECRETOS NA PRISÃO DO JAILBREAK (NUNCA VISTO) 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=_4LQia3zFbg
    * CURIOSIDADES DO JAILBREAK | OQUE TEM DEPOIS DO FINAL DA PISTA ? 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=bpDZChI00mI
    * O PINGUIN POLICIAL HACKER NO JAILBREAK (ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=JNS-EEbFdpY
    * CURIOSIDADES DO JAILBREAK | OQUE TEM DENTRO DAS NOVAS CONSTRUÇÕES ? 😲
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=jbQ23Z-Uhrw
    * SOU O MAIOR PINGUIN HACKER DO JAILBREAK(BUG SPEED ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=AZmKWNnKdeE
    * O PALHAÇO MAIS CRIMINOSO DO JAILBREAK (ROBLOX)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=XFFz_TgvrzM
    * COMO GANHAR MUITO DINHEIRO NO VEHICLE SIMULATOR - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=jXuxSpmR0uY
    * ROBLOX - COMO ESTA O WEIGHT LIFTING SIMULATOR 2 ??
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=I1q5pH_uEz8
    * ROBLOX - COMO E SER O MAIS FORTE DO MAPA ??(BOXING SIMULATOR 2)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=gIKytS9el8w
    * ROBLOX - SOU O PIOR ARREMESSADOR DE FACAS NO KNIFE CAPSULES(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=lghObuuLPbw
    * ROBLOX - PALHAÇO POLICIAL NO JAILBREAK(PRENDI TODO MUNDO?)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=7C-AV8i4k6o
    * ROBLOX - SOU UM GRANDE SAYAJIN NO DRAGON BALL Z FINAL STAND
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=C-8qVDEkx_o
    * ROBLOX - PALHAÇO MATANDO TODOS NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=yzYbxLwAGbQ
    * OS GUEST FORAM DESATIVADOS NO ROBLOX (EXPLICANDO O MOTIVO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=zjHpPmTIlWM
    * CUPHEAD NO ROBLOX (NOVO JOGO BETA)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=xcZhaAt5yuM
    * ME VESTINDO DE PALHAÇO NO ROBLOX (PALHAÇO IT??)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=u6xIWen3gEo
    * ROBLOX - BOB ESPONJA BOXEADOR NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=7KrRpYku5sI
    * ROBLOX - SLITHERT.IO DE DRAGÕES NO ROBLOX??(DRAGON RIDERS)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=NpWtIWssvjw
    * ROBLOX - O MAGRELO SUPER FORTE - BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ECAdKONJgx4
    * EGUINHA MIJOLETA VERSÃO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Uz_DlC4q1XM
    * ROBLOX - MELHOR JOGO DE TIRO DO ROBLOX??(WILD REVOLVERS)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Syss0lLy58E
    * ROBLOX - SIMULADOR DE DEUSES NO ROBLOX - GOD SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=GP8dCsp3yPs
    * ROBLOX - O GORDO BOXEADOR NO EATING SIMULATOR
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=A2jXwsBw4aE
    * ROBLOX - O CARA TEM QUASE 2 MILHÕES DE FORÇA NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=b1oAA1osD1A
    * ROBLOX - MALHANDO AO AR LIVRE NO MUSCLE BUSTER(NOVO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=omcecq1CJak
    * ROBLOX - MALHANDO NA PRISÃO - PRISONER SIMULATOR 2(NOVO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=GK1a6xREDQ0
    * ROBLOX - ESCAPE DA ESCOLA (ESCAPE SCHOOL OBBY)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=sWSXFr_yO84
    * ROBLOX - ESTOU COM MUITA DIARREIA NO EATING SIMULATOR
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=EDhJS-g3IwM
    * ROBLOX - AGRADECIMENTO AOS 10K DE INSCRITOS !!OBRIGADO FÁMILIA S2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=0lSQnsHb8eA
    * ROBLOX - SOU O HOMEM BOMBA NO NINJA SIMULATOR BETA
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=GJjV1-8nNcA
    * ROBLOX - DESBLOQUEANDO NOVOS PODERES DE GORDO NO EATING SIMULATOR
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=z8u5wngb34M
    * ROBLOX - MEDITAR NO MASTER DOJO FICA FORTE MAIS RÁPIDO?(NINJA SIMULATOR BETA)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=EgWibhxx630
    * ROBLOX - SOU UM ANJO LINDO NO ANGELS VS DEMONS SIMULATOR(NOVO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=XQ_sijXtE3A
    * ROBLOX - FICANDO MUITO GORDO NO EATING SIMULATOR(NOVO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=yhBmiKdi4Tg
    * ROBLOX - 119K DE FORÇA E GRANDE CAMPEÃO NO RUMBLE NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=7CHrusIIq48
    * ROBLOX - RUMO AOS 200K DE FORÇA NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ahMH9L9tXlo
    * FAZENDO PARKOUR INCRÍVEIS NO ROBLOX (NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=2rp7X6IOFS0
    * ROBLOX - TREINANDO E MATANDO GERAL NO NINJA SIMULATOR BETA
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=DTRxr-AeG_8
    * ROBLOX - SOU UM GRANDE DEUS NO GOD SIMULATOR 2(NOVO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=gqpHO5LGbiE
    * ROBLOX - SPIRIT OF LIFE A ESPADA DO LV 500 NO NINJA SIMULATOR BETA
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=zWS0TvC5b2Q
    * ROBLOX - SOU UM GRANDE NINJA NO NINJA SIMULATOR BETA(NOVO JOGO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=C7h-3uONdiU
    * ROBLOX - MELHOR FORMA PARA FICAR FORTE NO NINJA SIMULATOR BETA
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=W0in84ZHp-A
    * ROBLOX - COMO CONSEGUIR FORÇA INFINITA NO BOXING SIMULATOR 2(NOVO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=BrAGgsj8f5w
    * ROBLOX - COMO CONSEGUIR FORÇA INFINITA NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=EAEph4C6p5c
    * ROBLOX - APARECEU MAIS UM HACKER NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=SpZ2W91TKPs
    * ROBLOX - VENCENDO TODOS NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=qxeMyTm_49k
    * ROBLOX - PILOTANDO O NOVO JETSKI  NO SHARKBITE
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=BdQhKWQzYHw
    * ROBLOX - PEGUEI 100K DE FORÇA NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=EkkGeBGNrZk
    * ROBLOX - SOU O MAIS FORTE DO SERVIDOR - WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=UHTyxN--ByA
    * ROBLOX - GASTEI 20MIL DE CASH EM SKIN DE LUVAS NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=L18Iuosc_bA
    * ROBLOX - SOU O MAIS FORTE DO SERVIDOR - BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=fHTe_av6xeM
    * UM DIA NA PADARIA FAZENDO BOLOS - BAKERS VALLEY ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=oPz-H6slSi0
    * ROBLOX - BURACO SECRETO NO JAILBREAK NOVO SEGREDO !!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=H79YUuKwjmE
    * ROBLOX - ENCHI O SERVIDOR DE PESO!! TROLANDO JOGADORES NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=YNb8ghsGQyM
    * ROBLOX - PEGUEI 50K DE FORÇA E FUI CAMPEÃO NO RUMBLE (BOXING SIMULATOR 2)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=sIzwJte4dhs
    * ROBLOX - APARECEU UM HACKER COM 4 BILHÕES DE FORÇA NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=CEIF9DZFXZI
    * ROBLOX - QUASE MORRI JOGANDO BENDY AND THE INK MACHINE
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=lQuelrEXvyM
    * ROBLOX - CÓDIGOS PARA GANHAR DINHEIRO NO VEHICLE SIMULATOR
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=zKJD225kT-Y
    * ROBLOX - COMO CONSEGUIR CASH E LUVAS COMUM OU PERSONALIZADAS NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=OQlrPESk4Q4
    * ROBLOX - SOBREVIVA AO ATAQUE DO TUBARÃO OU MATE ELE (SHARKBITE)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=44S1h1-xl68
    * ROBLOX - COMO FICAR FORTE MAIS RÁPIDO NO BOXING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ExY1nyfk9Ds
    * ROBLOX - PEGUEI 100K DE STRENGHT(FORÇA) NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=oo47aPCz_Zc
    * ROBLOX - ANDANDO NO CÉU NOVO BUG DO JAILBREAK
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=uFbdy59OPA4
    * ROBLOX - SERVIDOR DOS PESOS GIGANTES COM OS INSCRITOS NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Fbxb5_ciE2o
    * ROBLOX - SEGUNDO MUNDO NO WEIGHT LIFTING SIMULATOR 2 NOVO BUG??
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=XODAw4duriU
    * ROBLOX - COMO FICAR FORTE MUITO RÁPIDO NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=B_dAa1zYGk4
    * O JOGO MAIS ASSUSTADOR DO ROBLOX \#1
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=lMnM_ow6fJM
    * ROBLOX  -  HELICÓPTERO VS BUGATTI CORRIDA NO JAILBREAK
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=UJ9mV3bo634
    * ROBLOX - EM QUANTOS MINUTOS MATO 100 JOGADORES ?? NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=kyS59lvHWLE
    * ROBLOX - PEGUEI 30K DE STRENGHT(FORÇA) NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=0OZxhodwEbU
    * ROBLOX - PRIVILÉGIOS VIP NO WEIGHT LIFTING SIMULATOR 2 (OQUE O VIP DA??)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=klkZFkFYmFw
    * ROBLOX - COMO CONSEGUIR O PESO QUE CUSTA 200 ROBUX (AKIMBO WEIGHTS) NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=tRu1vTy0DYo
    * ROBLOX - NOVA ATUALIZAÇÃO NO WEIGHT LIFTING SIMULATOR 2 (OQUE HA DE NOVO ???)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=-2fTo2-5y78
    * ROBLOX - HELICÓPTERO DE GUERRA NO JAILBREAK ATIRANDO NOS CRIMINOSOS
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=P-NsWgfASoc
    * ROBLOX - O MAIOR CARA DO JOGO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=RRFrByzMx2U
    * ROBLOX TODOS OS CÓDIGOS DO JOGO ASSASSIN
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ZerSNlLtBrc
    * ROBLOX - COMO DIRIGIR UM CARRO E ATIRAR AO MESMO TEMPO NO JAILBREAK (NOVO BUG)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=Bz_guEQXe8k
    * ROBLOX TODOS OS CÓDIGOS DO MURDER MYSTERY 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=wADu3-f8OhM
    * VIRAMOS HUMANOS NO ROBLOX !! COMO ISSO E POSSÍVEL ??(Part.EmpireBlox)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=KE0TkuxQtio
    * GRANDE MISTÉRIO UM ENIGMA NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=sqN8lEZcgi0
    * ROBLOX - SERVIDOR PRIVADO(VIP) ABERTO NO WEIGHT LIFTING SIMULATOR 2
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=C8eof_AEeEE
    * ROBLOX - NOVA ACADEMIA PARA GIGANTES NO WEIGHT LIFTING SIMULATOR 2(SERVER VIP)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=IrF2C8cole0
    * 30 SERVIDORES PRIVADO(VIP) ABERTOS PRA VOCÊ FAZER DINHEIRO NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=PWJ--h-xaIA
    * SERVIDORES PRIVADO(VIP) ABERTOS NO JAILBREAK - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=2xX-1ge4AN4
    * COMO FICAR MUITO RÁPIDO NO WEIGTH LIFTING SIMULATOR 2 - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=7NsPFQIiMOw
    * SIMULADOR DE ACADEMIA NO ROBLOX - Weight Lifting Simulator 2(NOVO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=tN_kJ3uDlFk
    * INTRO DO CANAL (POR ENQUANTO)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=ssxBDQj5-GQ
    * MATANDO ZOMBIE NO ROBLOX MATEI MUITOS SEM MORRER \O/
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=0-z9WyZcHiA
    * OQUE ACONTECE QUANDO UMA BOMBA ATÔMICA E ACIONADA EM UM JOGO DO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=dru_m0sLxpU
    * COMO MUDAR O CURSOR DO MOUSE NO ROBLOX (PERSONALIZAR SETA)
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=PEhsVxPQbTA
    * COMO CRIAR UM JOGO NO ROBLOX NOÇÕES BÁSICAS !
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=uG-rCIoqydw
    * PORQUE MUDEI O NOME DO MEU CANAL ?? :O
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=hDCPyZLCUIo
    * ROBLOX - ATUALIZAÇÃO NO JAILBREAK + NOVA PINTURA E NOVA FORMA DE ESCAPAR DA PRISÃO !!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=yNasB0vUVfs
    * DECORANDO MEU APARTAMENTO NO JAILBREAK
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=42dG-ISQe74
    * PRIMEIRO JOGO A SER CRIADO NO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=pyanhrJHgY0
    * PEDRA FLUTUANTE DENTRO DA CACHOEIRA COMO ISSO E POSSÍVEL ?? JAILBREAK ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=R9yUHfM5jI8
    * ROBLOX - ESCONDERIJO SECRETO NO JAILBREAK QUE VOCÊ NÃO SABE QUE EXISTE !!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=hAZk2omu9jI
    * ROBLOX - COMPREI A SWAT E PRENDI TODO MUNDO NO JAILBREAK
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=iJ1psdGSoNg
    * VIDA DE POLICIAL NO JAILBREAK !! PRENDI TODO MUNDO - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=vfI760KVyrs
    * TENTANDO ME VESTIR IGUAL AO GUEST666 NO ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=QcGBxTLAIdY
    * ACHEI UM NEGOCIO ESTRANHO ENQUANTO PASSEAVA DE HELICÓPTERO NO JAILBREAKBETA - ROBLOX
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=vUBicXdMdrA
    * UMA HISTORIA QUE FALA SOBRE O ASSUSTADOR GUEST 666 ‹ Roblox Mistérios ›
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=KymSQXX_x6M
    * CUIDADOO 😲!!! NOVOS BOTS NO ROBLOX SE VOCÊ VER UM DELES IGNORE-OS !!!
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=S2lqwN5si8I
    * ASSUSTADOR JOGO DO JOHN DOE AINDA EXISTE ?? ‹ Roblox Mistérios ›
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=YtgwGSeSHr0
    * COMO CRIAR UMA T-SHIRT (CAMISETA GRÁTIS) NO ROBLOX \#ATUALIZADO
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=aENGhK_nI30
    * DIMENSÃO DOS HACKERS QUE FORAM BANIDOS !! MUITO ASSUSTADOR :O ‹ Roblox Mistérios ›
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=6WwvMjm9kgw
    * APARECEU UM BONECO BRANCO E SUMIU NA DIMENSÃO DOS BANIDOS :O ‹ Roblox Mistérios ›
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=og0Tq0a2JW4
    * HACKER BOB ESPONJA ELE AINDA EXISTE ?? :O ‹ Roblox Mistérios ›
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=er8bmm9FcD0
* Jie GamingStudio (jieeroblox)
  * Information Collection
    * Jie 100k ROBLOX Art Contest(CLOSED)
      * Description references the data collection website easyrobux.today.
      * URL: https://www.youtube.com/watch?v=b1c0hl_WBiM
* Karola20 (karola20YT)
  * Information Collection
    * MEEPCITY BAILE DÍA DEL AMOR CON MIS ALUMNOS💖PROPUESTA DEL PROFESOR😱💖- ROBLOX
      * Description references the data collection website rbxtrade.com.
      * URL: https://www.youtube.com/watch?v=ZoDzvJ-dCDc
* Kavra (Kavra)
  * Non-Giftcard Robux Giveaways
    * DRAWING & VIDEO CONTEST (300K SUBS)
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=elG3Tjy7UVs
    * DRAWING & GFX CONTEST (WINNERS!!!)
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=X_i7rcVVtMg
* Keisyo (Keisyo)
  * Non-Giftcard Robux Giveaways
    * GETTING REVENGE ON BULLY AS PRINCIPAL / Roblox High School / Roblox Principal / Roblox Funny Moments
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=Hr2iIpK09Wo
    * TROLLING EVERYONE WITH ADMIN COMMANDS // Roblox Laundromat // Roblox Admin Command Trolling // Funny
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=m3Ebp6u-0RA
    * I AM SANTA'S NEW ELF! 🎄🎅 // Roblox Santa's Workshop // Roblox Funny Moments // Roblox Roleplay
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=GkxnkqcsQ5c
    * NOOBS RUIN ODER PARTY // Roblox Meepcity // Roblox Funny Moments // Roblox  Online Dating
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=eJxpBfC757s
    * WE'RE MAKING OUR FIRST FRIENDS // Roblox Royale High School \#3 w/ Cybernova & Cheridet
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=wOZdJI5MKxs
    * DONATING 100K TO STRANGERS IN BLOXBURG 🎁🎄 // Roblox Bloxburg // Roblox Christmas // Bloxburg House
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=ExgopIeUs2s
    * WEIRD MOM KILLS AND ABANDONS ME 😢 // Roblox Adopt me // Roblox Adopt Game // Roblox Funny Moments
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=UA9jFMe71DU
    * BEAUTIFUL CHRISTMAS MANSION // Roblox Bloxburg Subscriber House Tour //  Bloxburg House Tour
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=Mc-Y2L7yJR4
    * BOYFRIEND DEFENDS ME FROM MY BULLY // Robloxian High School // Roblox Bully // Roblox High School
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=yO8_rvyVWnA
    * TRAPPED IN SCHOOL WITH SLENDERMAN AND JASON 😱 // Robloxian High School // Roblox Creepy Pasta
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=qOqeAlfhXLA
    * KIDNAPPING FANS WITH ADMIN COMMANDS AND GETTING KIDNAPPED IN ROBLOX 😱 // Roblox Adopt and Raise
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=iqqcHLRqBI8
    * SPENDING OVER 25000 GEMS // GETTING A SKIRT & DECORATING OUR LOCKER //  Roblox Royale High School \#2
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=VTtVC02RJaA
    * BULLIED BECAUSE I'M A YOUTUBER? 😟 // Roblox Runway Rumble // Roblox Funny Moments // Roblox Bully
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=DC4jIOsrM1k
    * LIFEGUARD WANTS TO DATE ME & GUEST 666?! 😱 // Robloxian Waterpark // Roblox Online Dating // Funny
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=1Tzg_H89AnU
    * CRAZY HACKER ATTACK ON HOSPITAL & SAVING LIVES // Roblox Hospital Life 2 // Roblox Funny Moments
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=7VCeVxKJvz4
    * WE MADE HIM CRY :( // Roblox Sad Story // Roblox Working in a Cinema // Roblox Funny Moments
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=i9PocA7B5Dc
    * CREEPY DATER WON'T LEAVE US ALONE // Roblox Funny Moments // Roblox Creepy // Roblox Online Dating
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=CQ9j5GdAJ9E
    * BULLY CHEATS ON HIS BULLY GIRLFRIEND & HUGE BULLY FIGHT // Robloxian Life // Roblox Funny Moments
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=EKevJjx8j-I
    * EVIL MOM KILLS ME // Roblox Laundromat // Roblox Funny Moments // Roblox Online Dater // Adopt a kid
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=HbA6ljsWRH4
    * MY FIRST DAY // Decorating my Dorm // Roblox Royale High School // Roblox School // Funny Moments
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=pOA6JECSr6w
    * CREEPY MANAGER!!! BUT WE MADE HIM QUIT // Roblox Pizza Place // Roblox Creepy // Roblox Online Dater
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=moCmW5mDfwo
    * BULLIED AS A MINION IN ROBLOX HIGH SCHOOL // Roblox High School // Bully Story // Roblox Minion
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=XCUAswuCf2A
    * WEIRD ONLINE DATER ROOMATE // Roblox High School Dorm Life // Roblox Weird // Roblox Funny Moments
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=M9EJ3kGaFhQ
    * CATFISH PRANK ON ONLINE DATERS // Robloxian Highschool // Roblox Funny Moments // Roblox Trolling
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=NMtAQQ3EE8g
    * UNICORNS MAKE ONLINE DATER LEAVE PARTY // Roblox Unicorns // Roblox Meepcity // Roblox Funny Moments
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=U6qxRjXNCcM
    * CREEPIEST BOSS IN ROBLOX // Roblox Bikini Bottom // Roblox Creepy Boss // Roblox Online Dater
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=29A7c8fLF2o
    * THE CRAZIEST GAME IN ROBLOX // I ACCIDENTALLY GRIEFED THE SERVER // Robloxian Town // Roblox Funny
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=dVPfYJdIbPc
    * CREEPY ONLINE DATER GIVES ME 100K // Roblox Bloxburg // Roblox Online Dater // Roblox Creepy
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=skqLTWtJg6o
    * BOYFRIEND TAKES ME TO PROM // ROBLOX PROM GONE WRONG // Roblox Prom // Roblox Boyfriend // Roleplay
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=fQwAdoLZosI
    * TROLLING PEOPLE AS A TURKEY FOR THANKSGIVING // Roblox Trolling  // Roblox Funny Moments
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=skfoGY-5b2M
    * OWNER TROLLS ME WITH ADMIN COMMANDS // Roblox Waterpark // Roblox Admin Trolling // Funny Moments
      * Link references a Robux giveaway through group funds
      * URL: https://www.youtube.com/watch?v=eeb7HEchoj4
* KreekCraft (StarCode_RealKreek)
  * Phishing
    * ROBLOX HACKING A HATERS FAN ACCOUNT AND TURNING THEM INTO A THICC GIRL!!
      * URL: https://www.youtube.com/watch?v=hqsYQu9twno
    * HACKING A FANS ROBLOX ACCOUNT AND GIVING THEM FREE ROBUX!! | Roblox LIVE 🔴
      * URL: https://www.youtube.com/watch?v=hC5BdAC2X8o
* Landon - ARKH 
  * Phishing
    * *buying fans robux .. ( roblox )*
      * *Logs into the fan's account to buy Robux, although it was to redeem the VISA giftcard.*
      * *URL: https://www.youtube.com/watch?v=9FRfBVEDEWc*
    * *CHRISTMAS HACKING A FANS ROBLOX ACCOUNT! \*FREE ROBUX SURPRISE\**
      * *URL: https://www.youtube.com/watch?v=pl_R32VfSpY*
    * *HACKING A FAN's ROBLOX ACCOUNT and HALLOWEEN PRANKING HIM! (Roblox Robux)*
      * *URL: https://www.youtube.com/watch?v=Yo0nFKXRlq8*
    * *HACKING A FAN's ROBLOX ACCOUNT and SPENDING ROBUX!!! (Roblox)*
      * *URL: https://www.youtube.com/watch?v=ANtXgWmNO2U*
    * *HACKING A FAN's ROBLOX ACCOUNT GONE WRONG .. \*HE CAUGHT ME\**
      * *URL: https://www.youtube.com/watch?v=oYpxpdjgY-E*
  * Non-Giftcard Robux Giveaways
    * *DONATING $60,000 ROBUX TO ROBLOX FANS ..*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=i1cj_n9yZ7k*
    * *free robux .. ( Roblox )*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=Azvi95olEqc*
    * *ROBLOX CHANGED THE TERMS OF SERVICE .. \*BECAUSE OF US\**
      * *Uses a new account and giving away the password to give free robux.*
      * *URL: https://www.youtube.com/watch?v=NCgtwqrIwZc*
    * *BYPASSING MY ROBLOX ACCOUNT PERM BAN!! ( Roblox Jailbreak )*
      * *Uses a new account and giving away the password to give free robux.*
      * *URL: https://www.youtube.com/watch?v=fSeftjImqtQ*
    * *roblox banned me permanently for this video .. (part 2)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=tA6bEFouhm8*
    * *roblox will ban me if they see this ..*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=-LnCwFja19M*
    * *JOIN THIS GROUP for FREE ROBUX .. ( ROBLOX )*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=xBF9z89ogyo*
    * *ROBLOX DELETED MY ACCOUNT AGAIN..*
      * *Uses a new account and giving away the password to give free robux.*
      * *URL: https://www.youtube.com/watch?v=MK2nKCcfvzg*
    * *Dear Roblox, Do NOT Watch This Video ..*
      * *Uses a new account and giving away the password to give free robux.*
      * *URL: https://www.youtube.com/watch?v=2-muEkmlzdE*
    * *1 MILLION FREE ROBUX .. ( Roblox )*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=tAfs1t-RezQ*
    * *FREE ROBUX 2018 .. ( Roblox )*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=3nCxELjvkz0*
    * *FREE ROBUX .. ( Roblox Christmas )*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=Am-MwWDVZuU*
    * *FREE ROBUX .. (Roblox)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=xTE5qNkDqf4*
    * *IF YOU CAN FIND HIM, WIN FREE ROBUX!!! (Roblox Jailbreak HIDE AND SEEK)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=ByQ08LtMnZQ*
    * *FREE ROBUX JAILBREAK HIDE AND SEEK .. (Roblox)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=Gpc3u2MUVes*
    * *i sent a fan 25,000 robux on accident .. (Roblox)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=S-ATSQM1GME*
    * *\*VOICE CHAT\* SURPRISING FAN with 35,000 FREE ROBUX! (Roblox Jailbreak)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=J8_aRqjUjRo*
    * *SURPRISING A FAN WITH 35,000 FREE ROBUX!!! (Roblox Jailbreak)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=k23t9yZ7WbI*
    * *kid wins 15,000 robux by cheating in jailbreak .. (roblox)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=8z18UL-IRTg*
    * *IF YOU FIND HIM, WIN 50,000 FREE ROBUX! (Roblox Jailbreak)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=G0G4Qm8-4_o*
    * *The BIGGEST FREE ROBUX GROUP in ROBLOX .. (Roblox Groups)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=i8vbfclLAKs*
    * *10,000 ROBUX FAN RACE in VEHICLE SIMULATOR! (Roblox Vehicle Simulator)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=3bz_DtAs9-k*
    * *GIVING A FAN 100k FREE ROBUX!!! (Roblox Groups)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=pGVF2WWyDDg*
    * *ACROSS THE MAP RACE for FREE ROBUX in ROBLOX JAILBREAK ..*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=2udekZjZU9M*
    * *This ROBLOX GROUP is GIVING MEMBERS FREE ROBUX!!!!*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=ldVEn74ndTU*
    * *IF YOU FIND HIM, WIN 10,000 FREE ROBUX! (Roblox Jailbreak)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=3h8384Gf3P8*
    * *\*\*ACCIDENTALLY\*\* GIVING A FAN 15,000 ROBUX .. (Roblox)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=4KIN7ERG_P4*
    * *SPENDING $100,000 ROBUX for 100k SUBSCRIBERS (Roblox)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=BnlN0m12GX0*
    * *JOIN THIS GROUP for FREE ROBUX!!! (Roblox Groups)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=MMg99bIUMvY*
    * *IF YOU SEE HIM, YOU WIN FREE ROBUX (Roblox JAILBREAK)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=C24TVONSpqc*
    * *IF YOU FIND HIM, YOU WIN FREE ROBUX!! (Roblox Jailbreak)*
      * *Uses a Roblox Group with group funds to give away Robux.*
      * *URL: https://www.youtube.com/watch?v=TCoMoruGc2A*
* Linkmon99 (Linkmon99)
  * Information Collection
    * Guess My ROBUX to WIN IT ALL!!! (R$?!) - Linkmon99 Richest Roblox Player
      * Description references the data collection website rbxtoys.com.
      * URL: https://www.youtube.com/watch?v=325_ILeNJ40
    * Donating ALL MY ROBUX to Fans At VIDCON!! - Linkmon99 IRL \#18
      * Description references the data collection website rbxtoys.com.
      * URL: https://www.youtube.com/watch?v=lqwtj7bfh-I
  * Non-Giftcard Robux Giveaways
    * IF YOU FIND ME, WIN 15,000 ROBUX!! (Fan HIDE & SEEK Challenge) - Linkmon99 ROBLOX
      * URL: https://www.youtube.com/watch?v=aQVzK3xhRAs
* locus (locus200k)
  * Non-Giftcard Robux Giveaways
    * I Trapped 100 Kids And Gave Them Swords... Winner Gets 1000 ROBUX! (Roblox)
      * Uses group funds to give away Robux for a competition.
      * URL: https://www.youtube.com/watch?v=_Sz6sWDSoqs
* LOGinHDi (L0GinHDi)
  * Non-Giftcard Robux Giveaways
    * If I Get Scared.. I Giveaway Robux!
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=mOPmy0Mgw-A
    * If I Rage Quit.. I Giveaway Robux!
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=A-gXdyn1j7w
    * IF I LAUGH.. I GIVEAWAY ROBUX!
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=Pg59O3UjmAo
    * 50,000 ROBUX GIVEAWAY!
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=ivZA173jzqM
* Lonnie (GPR3)
  * Non-Giftcard Robux Giveaways
    * GIVING AWAY FREE ROBUX TO FANS!! (Roblox) \*CLICK TO ENTER!!\*
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=NEu-7Z0vfTE
    * FREE ROBUX GIVEAWAY! \*CLICK TO ENTER\* (REAL)
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=0VwLM8MEcN4
    * Giving a Fan 10,000 ROBUX! \*YOU CAN GET SOME TOO!\*
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=SPRA4wgIGmo
    * FREE PRESENTS GIVEAWAY WINNER!!!
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=NWEzobZkrKk
    * FREE ROBUX GIVEAWAY WINNERS!!!!
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=JVemQZY5iro
* Lons Gamer (LonssGamer)
  * Information Collection
    * Roblox - Brincando de Carrinho Bate-Bate com inscritos no Lumber Tycoon 2!! (Valendo Robux)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=9diQlI93dnA
    * Roblox - ABRI MAIS DE 200K EM COFRES NO JAILBREAK!! (Será que tive sorte?)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=fela7wO9ClQ
    * Roblox - COMO ROUBAR O TREM NO JAILBREAK!! (Muito Fácil)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=dYa9SzTMk6I
    * MELHOR RÉPLICA DO MARIO 64 NO ROBLOX!! (Robot 64)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=6lZ4PeJQXrM
    * VIREI UM GODZILLA NO ROBLOX!! (Godzilla Simulator)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=023PVydZDjE
    * Roblox - CONSEGUI O "Bee Axe" (Lumber Tycoon 2)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=v9QNVIpRM_s
    * Roblox - OQUE VEM DENTRO DE CADA PRESENTE DA ATUALIZAÇÃO!! (Lumber Tycoon 2)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=AxoEYU2x5mg
    * Roblox - MOSTRANDO O CÓDIGO 8!! (Lumber Tycoon 2)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=vVnfW4CAkWA
    * Roblox - COMO CONSEGUIR O SUPOSTO "Bee Axe" (Lumber Tycoon 2)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=-Lc2nobh8E8
    * Roblox - MOSTRANDO O CÓDIGO 7!!, E EXPLICANDO OS CÓDIGOS 1, 2, e 3 (Lumber Tycoon 2)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=aHi3L6QUJZ8
    * Roblox - O PRESENTE DO LABIRINTO!! (Lumber Tycoon 2)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=8d3U25QHg_s
    * MELHOR RÉPLICA DO CLUB DAS WINX NO ROBLOX!!
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=TreMy4yhGN0
    * MELHOR REPLICA DE CUPHEAD NO ROBLOX!!
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=yGZyD7yMo2A
    * Roblox - DICAS DE COMO SE TORNAR O MELHOR NINJA!! (Ninja Assassin)
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=_QmSz1DVsqE
    * Roblox - FIZEMOS UMA SPOOK WOOD GIGANTE NO LUMBER TYCOON 2!!
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Mk-WIRWiiq0
    * Roblox - A CAIXA MAIS RARA DO LUMBER TYCOON 2!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=PjipkRCDeEY
* Lyna (Lynitaa)
  * Non-Giftcard Robux Giveaways
    * REGALO 100.000 ROBUX SI PIERDO ESTE RETO EN ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=ZNZLqUPWnfc
* MIKEYDOOD (IMMIKEYDOOD)
  * Phishing
    * HACKING MY VIEWER AND SPENDING ALL OF HIS ROBUX!!!
      * URL: https://www.youtube.com/watch?v=j3ao8hO0ANI
    * HACKING INTO MY FANS' ACCOUNTS?!
      * URL: https://www.youtube.com/watch?v=QQWTL6qob4M
  * Non-Giftcard Robux Giveaways
    * GIVING AWAY 15,000+ ROBUX FOR... (Roblox)
      * Video announces a Robux giveaway.
      * URL: https://www.youtube.com/watch?v=df0NiP-SRUQ
    * EASIEST WAY TO EARN ROBUX FROM ME!!! \*FREE ROBUX GIVEAWAYS\*
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=WZ7dbUBOePA
* MuneebParwazMP (MuneebParwazMP)
  * Phishing
    * (GONE WRONG) HACKING A FAN AND GIVING THEM FREE ROBUX!!! | Roblox
      * URL: https://www.youtube.com/watch?v=NJzfMDUMmMw
  * Non-Giftcard Robux Giveaways
    * (Giveaway Ended)
      * Video shows giving away Robux using group funds for specific amounts based on paid access rates for games.
      * URL: https://www.youtube.com/watch?v=0BK6m8uRrNU
* MyUsernamesThis (UseStarCode_BACON)
  * Non-Giftcard Robux Giveaways
    * HIDE and SEEK on NEW MAP for 3000 ROBUX! | Roblox Jailbreak
      * Uses group funds to give away Robux for a competition.
      * URL: https://www.youtube.com/watch?v=5UQfA8iJm8s
* NikTac 
  * Non-Giftcard Robux Giveaways
    * \*SECRET\* HALLOWEEN BEES & EVIL BEAR COMING IN Roblox Bee Swarm Simulator!? (Update Theory)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=C88HJrMvk7k
    * LEGENDARY ROBLOX ICE CREAM SIMULATOR CODES \*INFINITE SCOOPS\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=licG1WyF4BQ
    * WORLDS BEST PLAYER SHARES SECRET TIPS & GLITCH!! - Roblox Super Power Training Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=8R5vUyuE9_E
    * WORST NOOB PET WITH THE ROBUX HAT! (GAME BREAKING!) - Roblox Pet Simulator (Hats Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=9fS67A74_xA
    * We found a GAME BREAKING Roblox Pet Simulator GLITCH... (Infinite Damage!) - Hats Update
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=YlvfLKwcc_w
    * UNLOCKING THE RAREST \*NEW\* UPDATE HAT ON ROBLOX PET SIMULATOR (Robux Hat!)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=tpSo81Vyt38
    * UNDERCOVER NOOB TAKES CONTROL OF SERVER!! - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=PskCVgUINHI
    * REBIRTH GLITCH, SKY GYM & TOP ON STRENGTH LEADERBOARD IN ROBLOX WEIGHT LIFTING SIMULATOR 3
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=CI5vStdQEKU
    * Strength Code & OP Strength Reached IN Roblox Weight Lifting Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=XxisHB3xYEM
    * MAKING MY ACCOUNT \*INVINCIBLE\* (FINAL SKILL!)  - Roblox Super Power Training Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=1M1ZSnjGoZQ
    * REACTING TO MY OLD CRINGEY VIDEOS! \*BAD\* (100K Special)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=46pfEKLM_u4
    * \*NEW\* SECRET SUN BEAR QUEST CODES IN ROBLOX BEE SWARM SIMULATOR
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=GEjubazPaJs
    * COMPLETING ALL WITCH UPDATE QUESTS (SECRET REWARD!) - Roblox Mining Simulator (Halloween Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=5kMsDO9YwyI
    * UNLOCKING VOLCANO AREA & BECOMING A POWERFUL SUPERHERO (Roblox Super Power Training Simulator)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=swncWfh83hY
    * \*NEW\* HALLOWEEN UPDATE CODES & ITEMS! - Roblox Mining Simulator (Haunted Pack)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Qsuz57YX7G0
    * NOOB WITH RAREST POSSIBLE PET!! (RAINBOW CORE SHOCK!) - Roblox Pet Simulator \*INSANE\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=YI6F1rfetBY
    * ENTERING \*SECRET\* MAP LOCATIONS (TORNADO ZONE!)  - Roblox Super Power Training Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=E_h59jTBksc
    * UNLOCKING CORE SHOCK PET & NEW CODES IN ROBLOX PET SIMULATOR (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=SXNTALzdQxg
    * 26 GODLY \*NEW\* ROBLOX BEE SWARM SIMULATOR CODES! (OP 10x FIELD BOOST!)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=IpYTOXQ544s
    * NOOB WITH MAXED RAINBOW CORE! (PRO In 50 SECONDS!) - Roblox Pet Simulator \*INSANE\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=TOmVJ3KEiv8
    * CATCHING \*INSANE\* PET SIMULATOR DUPER!! - INFINITE Rainbow Pets (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=zZrgbQXD9Fk
    * I GOT TRADED THE BEST POSSIBLE TIER 16 PET ON ROBLOX PET SIMULATOR (RAINBOW C0RE!)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=PVMFkzS8FMQ
    * UNLOCKING BEST \*NEW\* RAINBOW CYBORG PETS! (TIER 16!) - Roblox Pet Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=P3k8S0FPV-8
    * SECRET TIER 16 PETS AND NEW UPDATE AREA! \*RAREST PETS\*- Roblox Pet Simulator (Leaks)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=104cnuyW0jY
    * 10 \*NEW\* YOUTUBER UPDATE CODES ON ROBOX BEE SWARM SIMULATOR (New Items!)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=BziAJpoVbF4
    * TRADING A NOOB GODLY DREAM PETS! (3x Rainbow Mortuus!) - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ZHibjuJf9rA
    * \*SECRET\* WATERFALL BASE DISCOVERED (FREE EGGS!) - Roblox Dragon Keeper Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=bQmIgXar_3c
    * NEW \*MYTHICAL\* CRYSTAL UPDATE CODES & AREA IN ROBLOX MINING SIMULATOR (New Items)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=G-D3nlb1gkE
    * 2 \*NEW\* SECRET CODES & INSANE GLITCH DISCOVERED! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=bAh8d2__Y8s
    * UNLOCKING \*NEW\* MYSTIC TIER GODLY DRAGON! - Roblox Dragon Keeper Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=elA4FrbUhiM
    * TRADING NOOB \*NEW\* RAREST PET IN PET SIMULATOR! \*INSTANT PRO\* (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=zkmvUF-bDy0
    * BUYING GODLY DOMORTUUS PET, CODES & RAINBOW UPDATE ON ROBLOX PET SIMULATOR
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=qYRY-c9NK10
    * \*NEW\* MYSTERY TIER 16 PETS IN ROBLOX PET SIMULATOR! (Update leak!)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=SmoxO-FVRSo
    * I GAVE 20 GODLY DOMINUS PETS TO NOOBS... (140k RBX) - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=91E8Wmm0zsY
    * 5 \*NEW\* NIGHT UPDATE CODES RELEASED ON ROBLOX BEE SWARM SIMULATOR (Free Items)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=pijUy2lcKW8
    * I DEFEATED THE DIAMOND TIER SPROUT! \*FREE STAR EGGS!?\* - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=fna5o3gn1B4
    * \*NEW\* GODLY DOGGO BEE UNLOCKED & VICIOUS BEE! - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ClcE-Q_Kfq8
    * EASIEST WAY TO GET STINGERS & MOON CHARMS! - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=3-R67IaWmWs
    * ALL \*NEW\* UPDATE CODES ON ROBLOX BEE SWARM SIMULATOR (\*FREE ITEMS\*)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Q8nrQCTwkP8
    * FULL \*NEW\* BEE SWARM SIMULATOR UPDATE REVIEW & OWNER LEAKS! (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=QvjAwKNSStg
    * MAKING A NOOB ACCOUNT \*INSTANTLY\* A MASTER!! (LEGIT) - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=VEMerM_WT8o
    * UNLOCKING \*LEGENDARY\* NEW UPDATE CODES AND AREAS IN ROBLOX DESTRUCTION SIMULATOR!
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=x-ttdXs4Lv8
    * \*NEW\* UPDATE & CODE LEAKED BY ROBLOX DESTRUCTION SIMULATOR OWNER!
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Fj-uL18HDdA
    * I EQUIPPED 1,000 TIER 15 PETS THEN CRASHED THE GAME...!! - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=xayP4TUlqQM
    * Glitching Into The TOP AREA as a NOOB! - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=bPuxqppi_kg
    * PET SIMULATOR \*OWNER\* MESSAGED ME THEN I WON THIS!... (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=d8qfBMA__fQ
    * BUYING 30 MYSTERY TIER EGGS ON ROBLOX PET SIMULATOR! \*WORTH IT?!\* (Candy Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=66cyGTFgK4Q
    * BUYING \*100 TIER 15 PETS!\* IN ROBLOX PET SIMULATOR!! (CANDY UPDATE)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=DpLZhZwhf-s
    * OWNER LEAKS \*NEW\* CANDY UPDATE INFO & PETS!! - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=bVRNJ4smCC8
    * NOOB VS PRO VS MASTER - ROBLOX DESTRUCTION SIMULATOR VERSION! \*EPIC!\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=9oPkULAsoKk
    * DESTROYING THE ENTIRE MAP WITH ONE BLACK HOLE!! \*INSANE\* - Roblox Destruction Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=JG_mTBWIYOI
    * BUYING \*INSANE\* BLACK HOLE PACK IN ROBLOX DESTRUCTION SIMULATOR  (MAX LEVEL!)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=C9VhU1y1iR0
    * \*NEW\* NUCLEAR ROBLOX DESTRUCTION SIMULATOR CODE & ITEMS! \*INSANE\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=p9oRNZQOX0M
    * THE OWNER GAVE ME SECRET \*MASTER TIER PETS\* (INSANE!) - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=6VNLZSn5QzY
    * GRINDING FOR \*NEW\* GODLY TIER UPDATE BEES!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=AZ3VTKt2oLQ
    * I EQUIPPED 1,000 TIER 14 PETS THEN THIS HAPPENED... \*INSANE\* - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=42ZeMXl76MM
    * OPENING 75 INSANE \*TIER 14\* UPDATE EGGS (GOLD PETS!!) - Roblox Pet Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=79hVbHvLQoI
    * \*SECRET\* UPDATE MOON CHEST DISCOVERED!! (GOD PETS!) - Roblox Pet Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=bJ14kV2vAKw
    * \*NEW\* UPDATE BEARS LEAKED BY OWNER & ALL INFO! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=HHdTzcb-i8Y
    * SPENDING \*1 TRILLION COINS\* ON 250 TIER 12 GOLD PETS!! - Roblox Pet Simulator \*INSANE\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=P0E4dKMmflI
    * WORLDS BEST POSSIBLE PLAYER ON ROBLOX PET SIMULATOR!!? (ALL GOLD PETS!!)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=587ePtmyRvY
    * REACTING TO \*INSANE\* INFINITE PET HACKERS ON ROBLOX PET SIMULATOR! (GODLY)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=OilrTZ-Tk0c
    * ALL UNTOLD GIFTED UPDATE SECRETS, CODES & LOCATIONS! - Roblox Bee swarm Simulator (Update Recap)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=TvasesFJMPw
    * NOOB VS PRO VS MASTER - ROBLOX PET SIMULATOR VERSION! \*EPIC!\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=vcKRDL-5Wyk
    * GIVING RANDOM PLAYERS \*ADMIN\* TROLL IN ROBLOX PET SIMULATOR!?
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=R85_mzQwS9s
    * TRADING \*NOOBS\* INSANE GOLD PETS IN THE GAME! - Roblox Pet Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=eVAiKZmtn6I
    * UNLOCKING INFINITE \*NEW\* GOLD PETS!! (INFINITE PETS!) - Roblox Pet Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=lGzYJ2muHtc
    * BUYING \*INSANE\* INFINITE PET GAMEPASS!! (40k RBX!) - Roblox Pet Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=zZXavSYcfSI
    * SECRET \*NEW\* GIFTED UPDATE CODE RELEASED! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=2Pnb6dhrxRg
    * GODLY SPACE BOSSES & NEW AREA UPDATE?! \*THEORY\* - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=m09RQLhzLlw
    * FIND ME AND YOU GET FREE STAR EGGS!! (\*LEGIT\*)... - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Je0n4bG2-Qg
    * \*LEGIT\* FREE BOSS GAMEPASS & ALL ROBLOX JAILBREAK UPDATE ITEMS! (Grenades, rocket launcher & More)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=OsCfoPvYgrg
    * OPENING\* GODLY\* TROPHY UPDATE BOXES!! - Roblox Egg Farming Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=AJ_omUWt1Fw
    * ALL \*NEW\* UPDATE LEAKS (OWNER SECRETS!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=EI72wWTqFOw
    * WORLDS BEST PLAYER SHOWS SECRET TIPS! - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=vI5Bd4zUsgY
    * UNLOCKING \*THE BEST PET\* IN THE GAME!! (TIER 12!) IN ROBLOX PET SIMULATOR (UPDATE)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=P4du9u3M1Kw
    * \*NEW\* INSANE UPDATE AREA UNLOCKED IN ROBLOX PET SIMULATOR \*BILLIONS\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=7XsaNetHv9U
    * COMPLETING \*ALL\* SIR MINESALOT UPDATE QUESTS IN ROBLOX MINING SIMULATOR \*MYTHICAL CHALLENGE\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=cJUVEBAoaCk
    * ROBLOX PET SIMULATOR \*NEW\* SECRET TIER EGGS LEAKED INGAME! (UPDATE NEWS)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=nCBAJoEopuA
    * FINDING & OPENING THE SECRET GOD CHEST! \*BILLIONS!\* - Roblox Pet Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=vrh9JSyoV-w
    * ALL CODES THE OWNER \*SECRETLY\* HID FROM US!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Pipn-KCDB78
    * UNLOCKING \*ALL\* POSSIBLE PETS IN ROBLOX PET SIMULATOR (TIER 10 EGGS!)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=U7Q47l5R4dk
    * ANSWER THIS CORRECT AND YOU GET 1 BILLION HONEY! - Roblox Bee Swarm Simulator w/OpaOsiris
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=gqFDn9GAttE
    * ROBLOX PET SIMULATOR \*UNLOCKING ALL AREAS\* & DOMINUS PET!
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=RoF3Hr7-fL8
    * HOW TO \*INSTANTLY\* GET FREE STAR EGGS IN BEE SWARM SIMULATOR (SECRET!)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=TvgDSrnHYHY
    * \*FREE LIGHT PACK\* & QUESTS UPDATE CODES IN ROBLOX MINING SIMULATOR!
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=yzDomgCMRGI
    * THE OWNERS FINAL QUEST... (GIFTED REWARDS!?) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=bK_LFvyM1i0
    * IS THIS THE NEXT ROBLOX BEE SWARM SIMULATOR...?
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=oDr3zqr_gwA
    * WHAT HAPPENS PAST LEVEL 2500...? (BEST PLAYER!) - Roblox Egg Farming Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=OyDynDnW9g4
    * ALL 20 \*ACTIVE\* GIFTED UPDATE CODES! (NEW CODES!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=-kYZSpN-O3g
    * 9 NEW LOCATIONS TO GET GIFTED BEES & EGSS! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=DGHsQaogBHA
    * 10 \*NEW\* GIFTED UPDATE CODES ON BEE SWARM SIMULATOR (Legendary Codes)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=jeaVuZ-RMkI
    * OWNER GAVE ME A \*SECRET\* GODLY CODE! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=qBgSQgMei08
    * \*FREE MYTHICAL\* UPDATE ON ROBLOX MINING SIMULATOR (NEW CODES)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=A4sFMU3RMX0
    * \*NEW\* UNKNOWN GIFTED EGG BOSSES! (FREE ITEMS!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Pk6WT8YGHXI
    * WORLDS BEST PLAYER SHARES SECRET TIPS!! - Roblox Egg Farming Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=0VuoGEYYANM
    * EASIEST WAYS TO GET GIFTED EGGS! (UPDATE CODES!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=DR12wopmHhw
    * COMPLETING OWNERS \*SECRET\* QUEST! (GIFTED EGG) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=VCHaaNtOy90
    * \*MYTHICAL\* BLACK AND GOLD EGGS! (NEW MOON LAND!) - Roblox Egg Farming Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=YJtqxdLc5TQ
    * UNLOCKING \*GOD TIER\* LEGENDARY ITEMS!! (UPDATE!) - Roblox Egg Farming Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=J6Tykwd3i90
    * HOW TO \*GLITCH\* INTO THE LEVEL 30 SECTION!! (SECRET) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=QAUqz1deOQA
    * DEFEATING GODLY CHICKEN BOSS! (MYTHICAL WEAPONS) - Roblox Egg Farming Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ZTh5MVSHYU0
    * COMPLETING FREE GIFTED BEE QUESTS! (MOTHER BEAR!) - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=8sIH0CjUi8M
    * ALL \*NEW\* SECRET GIFTED EGG & TICKET LOCATIONS! (FREE) - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=aQU23FDOAPo
    * \*SECRET\* DIAMOND EGG GOD BOSS!! (FREE ITEMS) - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Id-7UbCAJB8
    * \*SECRET\* GIFTED EGG BOSS FIGHT!! (TUNNEL BEAR!) - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=052X_Nt2FgU
    * HOW TO GET FREE GIFTED BEES \*LEGIT\* - Roblox Be Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=LQGtgFQzMZk
    * THE \*SECRET\* VOID REALM DISCOVERED IN ROBLOX MINING SIMULATOR (MUST FIND)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=qsK8Jm8s5hw
    * FREE GEM CODE & NUCLEAR EXPLOSIVE C4!! - Roblox Moon Miner 2 (Simulator)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=S1FaAcllMPY
    * \*FASTEST\* WAYS TO DOUBLE BACKPACK SPACE!! - Roblox Bee Swarm Simulator (Tips)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Kc2zMchFhrk
    * ALL \*LEGENDARY\* ROBLOX FARMING SIMULATOR CODES (NUKE UNLOCKED)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Fuj54sHgoEU
    * \*SECRET\* YOUTUBER BEES & SPACE ARENA! (SHOWCASE) - Roblox Bee Swarm Simulator (Showcase server)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=KcQnwgD76Z8
    * NEW MYTHICAL UPDATE CODES & \*GOD\* ITEMS!! - Roblox Mining Simulator Codes
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=WuDIlxxenyQ
    * UNLOCKING \*GOLDEN HOE\* & ALL AREAS!! - Roblox Farming Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=lpzRFx7xQB0
    * NEW \*SECRET\* UPDATE BEAR LEAKED (+GOD CODE!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=tnjcB60OqJI
    * TOP PLAYER SHOWS OFF SECRET CODE!! \*NEW\* - Roblox Bee Swarm Simulator w/OpaOsiris
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=TLgkzm5k5U8
    * UNLOCKING EVERY PET IN THE GAME! (MAX) - Roblox Beach Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Vwz5PUsCuB8
    * \*ADMIN\* MAKES ME INTO A CUSTOM BEE!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Lqw6JDW-FkQ
    * \*NEW\* ROBLOX ISLAND ROYALE CODE! (CLUTCH VICTORY!) - Roblox Fortnite
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=jVl--8jIYOo
    * THE UNTOLD SECRETS OF MINING SIMULATOR \*MUST KNOW\* (Codes) -Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=UNKJB7MUkvw
    * \*SECRET\* 30+ NEW UPDATE BEES LEAKED!!? - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=qF62LQ5gvh0
    * NEW ROBLOX ISLAND ROYALE CODE! \*FREE BUCKS\* - Roblox Fortnite
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=wqFYHfh6m68
    * (NEW) 20 LEGENDARY ROBLOX BEE SWARM SIMULATOR CODES \*NEW BEES\* (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=L8kuPH_qFbQ
    * KING ANT BOSS & ALL SECRET UPDATE INFO!! - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=OvgD3wbn4YA
    * BUYING \*GODLY\* ELECTRIC HAMMER + NEW CODES!! - Roblox Mining Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=5yOGDEof0Io
    * NEW UPDATE, CODES & INSANE VEHICLE!! - Roblox Fire Fighting Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=SQvMJCptTio
    * ATLANTIS UPDATE \*CODES\* & ALL ITEMS!! (INSANE!) - Roblox Mining Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=8vmLVe5wTTQ
    * GETTING \*NEW\* MYTHICAL BEE FOR FREE!? \*WAGER\* - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=aBpacNFKAk4
    * 35 \*SECRET\* WAYS TO GET FREE ROYAL JELLIES & TICKETS! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=yt-gV6Ff9Mo
    * NEW MAP & SECRET LEVEL 30 FLOWER ARENA! (UPDATE LEAKED) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=KYYaFD80CgI
    * 100+ LEGENDARY & MYTHICAL ROBLOX MINING SIMULATOR CODES \*MYTHICAL UPDATE CODES\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=1iu2pSrXY08
    * (CODES) BUYING THE LEGENDARY BLASTER TOOL!! (OP) - Roblox Fire Fighting Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=gXXlqKxN3ZI
    * NEW CODES MAKE YOU AN INSTANT PRO!! (Noob to Pro) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=aGg0x9LLUQI
    * (CODES!) INSTANTLY BUYING THE BEST ITEM (WORLD BACKPACK) - Roblox Fire Fighting Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=yfgnTVIi6ak
    * SUMMER UPDATE, \*CODES\* & FREE ITEMS! - Roblox Mining Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=0zqrnkcWVAE
    * \*NEW\* THE BEST POSSIBLE MINING SIMULATOR ACCOUNT (TOP PLAYER) - Roblox Mining Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=MwcVdgZpfpE
    * BUILD A BOAT \*UPDATE\* FLYING HOUSE (ROCKET FULED!) - Roblox Build a Boat For Treasure
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=2DHbM6Px2OQ
    * THE ULTIMATE \*SECRET\* HIVE SETUP! (MUST HAVE) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=zUOy2vUCsXA
    * UNLOCKING \*EVERY\* REBIRTH ITEM! (MAXED!) - Roblox Mining Simulator (Codes)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=eyLbzKFcmEY
    * HOW TO GET FREE MYTHICAL ITEMS! \*LEGIT\* - Roblox Mining Simulator (Giveaway)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=aqhbymprlu0
    * (NEW) 17 LEGENDARY BEE SWARM SIMULATOR CODES \*OP PERKS\* Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=yVCmd_FMfJE
    * REBIRTH IN 2 SECONDS! \*LEGIT\* (MOAB Explosive) - Roblox Mining Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ghtWDJkLXMc
    * MYTHICAL TIER UPDATE \*CODES\* & ALL ITEMS! - Roblox Mining Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=GtGYDTNQijw
    * SECRETS EVERY PLAYER SHOULD KNOW (CODES, ROYAL JELLYS TICKETS) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=K_AbupEQsuM
    * HOW \*OP\* IS THE FIRE BANE?! (INSTANT BILLIONS!!) - Roblox Mining Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=cnQ6ISZfN_I
    * ROBLOX NOOB VS PRO VS BILLIONAIRE - Roblox Bee Swarm Simulator \*FUNNY\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=-5qsyBE3YnQ
    * NEW \*FREE\* TICKET, BEE & ROYAL JELLY CODES! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=tLjj1XqBPII
    * UNLOCKING \*NEW\* CRIMSON & COBALT BEES (NEW TOOL) - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ezfDIq7sL7c
    * \*ALL NEW\* LEGENDARY ROBLOX MINING SIMULATOR CODES! (New)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=jcWIgg82Pus
    * ROBLOX BEE SWARM SIMULATOR WAGER \*FREE GUMMY BEE\* w/DefildPlays
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=FnkAz89GnMs
    * GODLY MINING SIMULATOR TOOLS (FIRE BANE + OMEGA CRATES) - Roblox Mining Simulator \*UPDATE\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=eHonjSkchO0
    * THE BEST MINING SIMULATOR ACCOUNT POSSIBLE!! \*TOP PLAYER\* - Roblox Mining Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=bJQly_VHyTs
    * ALL \*NEW\* BEE SWARM SIMULATOR CODES (GUMMY UPDATE CODES) | Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=fGFwwuwMnHE
    * NEW LEGENDARY CODES & ROBLOX CHALLENGE!! - Roblox Mining Simulator (Update) w/DefildPlays
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=2rx69zvJEfo
    * \*NEW\* GUMMY BEAR SHOWCASE (OP ABILITY!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Mmo1I5N55fA
    * FIRST MAXED MOJO GEAR ACCOUNT! (ALL ITEMS!) - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=gtfQE_5Geew
    * \*GODLY\* MOJO UPDATE ITEMS! (GOD ROCK!) - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=jHdx2fwJXoQ
    * \*NEW\* GUMMY UPDATE! (GUMMY BEE + BEAR) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=vcpnaepkuwU
    * LEGENDARY DINO UPDATE CODES!! (LEGENDARY EGG) - Roblox Mining Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=VOsMfiYsjLI
    * \*INSANE\* INVISIBILITY GLITCH LEAKED!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=zcwrs8bJEAY
    * \*NEW\* LEGENDARY BEE SWARM SIMULATOR CODES (BEAR BEE PERK)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=qvOm1Wcx26g
    * ARE ROBLOX MINING SIMULATOR CODES TOO OP?! \*SO INSANE\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=w6ZxtteLkK8
    * WORLDS BEST PLAYER SHOWS SECRET CODES!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ORSCItibx94
    * FAN DONATES 100K COINS (CRASHES SERVER!) - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=YvHQI5ruAyA
    * ALL \*NEW\* SECRET TICKET & ROYAL JELLY CODES!! - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=1NZ1hxS6cWQ
    * So I got Permanently Banned From Roblox...
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=1ynHJWNpblM
    * NEW LEGENDARY CODES, FOOD UPDATE, TURKEY BLASTER - Roblox Mining Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=dtOYpN4Sgnw
    * I CAUGHT A TOP PLAYER \*HACKING\*!!? - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=0vvcnC6bGsM
    * THE FASTEST REBIRTH EVER!? (OP TACTIC) - Roblox Mining Simulator (Codes)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=AyoVNCMgxZk
    * SECRET \*NEW\* UPDATE BEAR LEAKED! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=TY6EaWxHL6E
    * HOW TO GET THE TABBY BEE FOR FREE!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=jXyl-QWRW1M
    * THE WORLDS SAFEST BASE (100% UNRAIDABLE!) - Roblox Booga Booga (Emerald Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=jIpbg3PH24A
    * UNLOCKING \*NEW\* GODLY TABBY BEE!! - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Sg1-GiJWTIk
    * \*UNLIMITED\* EMERALDS!! (Massive OPENING!) - Roblox Booga Booga (Emerald Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=lGe4oaAoen8
    * SECRET EMERALD CAVES + GOD BAGS REMOVED (Future Updates) - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ZkUQaqEhMt4
    * USING YOUTUBE STATUS TO GET GODLY GEAR!! (WORKED!) - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=0uTdOnE2Kq8
    * THE BEST POSSIBLE ACCOUNT ON THE GAME!!? - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=rzuILsmDoZQ
    * GODLY \*DIMENSIONAL\* GEAR!! - Roblox Booga Booga & Big Booga Dig
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=zXqDpkw3Nyc
    * NEW \*SECRET\* BEES ACCIDENTALLY LEAKED!? - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=DXk17VWdFko
    * MY GAMING SETUP TOUR!! - 30K Special - (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=18RgZvoLwzI
    * UNLOCKING EVERY POSSIBLE BEE IN THE GAME!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=qKNv-rn-VUA
    * \*ALL NEW UNKNOWN SECRETS AND LOCATIONS!!\* - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=TJUQZl-WZGM
    * (INSTANT MILLIONS!) 60 MILLION BUCKO GUARD!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=AtuIJzq7dvY
    * TAKING OVER A TOP PLAYERS ACCOUNT!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=73cPVOugnEo
    * \*SECRET\* VOLCANO PARKOUR!! (TICKET LOCATIONS!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=0LOeFLOTPJA
    * BUYING THE OP \*NEW\* PHOTON BEE (UPDATE!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=wfjXOiMcb2U
    * HACKING A TOP PLAYERS ACCOUNT! (SECRET ITEMS!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Y9L0ZR28_2w
    * THE ULTIMATE ZOMBIE BASE DEFENCE!! - Roblox Island 2
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=yskSBNiwoWY
    * NOOB INVADES \*SECRET\* PRO FLOWER AREA!! - Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=30b3-BW6EtM
    * THE UNTOLD SECRETS OF BEE SWARM SIMULATOR!! (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Likk4UUeA-Q
    * \*NEW\* MILITARY HELICOPTER VS REGULAR HELICOPTER - Roblox Jailbreak (1 Year Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=UrEIPY-Akk4
    * UNDERCOVER PRO PLAYER TAKES CONTROL OF SERVER!!- Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=v7AKl6dFwhw
    * PLAYING WITH 4 PRO PLAYERS! (120 Bees!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Xe655apHKb0
    * NOOB USES INSANE HACKED CLIENT!! (SUPER OP!) - Roblox Bee Swam Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=UKH-sVI731Y
    * (Instant Honey!) $25,000,000 BEEKEEPER'S MASK!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=6xdju-BZuAw
    * INSTANT \*MILLIONS\* IN CANDY LAND DIGSITE!! - Roblox Mining Simulator (Codes)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=g6vMY2ho4F8
    * UNLIMITED OXYGEN GLITCH (MAXIMUM DEPTH!) - Roblox Scuba Diving Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=u7OSxx0-lXA
    * GOD BEETLE BOSS + SECRET ROYAL JELLY MAZE!! - Roblox Bee Swarm Simulator (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=6VrrC5yAL3Q
    * ALL NEW SECRET LOCATIONS AND AREAS!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=HRolxPYl1Nw
    * BUYING OP GOD BAG AND PICKAXE!! - Roblox Big Booga Dig (Simulator)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=bwYA8ZIkLMI
    * \*NEW\* BOOGA BOOGA GAME!! - Big Booga Dig Roblox (Simulator)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=sEGSJz9TXrk
    * 1 MILLION BELI IN 15 MINUITES! (Legit) - Roblox One Piece Bizarre Adventures
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=R4K6pfXe2mo
    * REMOVING ALL THE FLOWERS FROM THE MAP!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ecoK37UNgOQ
    * SECRET ALIEN BOSS EASTER EGG! (All Updates) - Roblox Jailbreak (Alien Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=R4QirQdrGo0
    * TOP PLAYER SHOWS NEW SECRET AREA!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=WtnNd-MIjow
    * RAIDING THE SECRET BARBARIAN BASE!! - Roblox Booga Booga (Dusk)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ANN7fdI0JmM
    * \*GLITCHING\* OUTSIDE THE MAP (EXPLOIT) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=x2i2O8WaaVA
    * NOOB VS PRO VS HACKER (Bee Swarm Simulator) - Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=i0bhtrXdSW8
    * \*SECRET\* GOLDEN EGG CAVE!! - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=jhXp6zRomyE
    * \*SECRET\* FLOWER AREA (MAX LEVEL!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=H2dwicdse9k
    * DISCOVERING NEW PLANETS \*999,999\* HEIGHT!  - Roblox Rocket Simulator 2
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=6VaraQQg3KM
    * \*HACKER\* KILLS THE WHOLE LOBBY!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=14qx3iPsiOc
    * UNLOCKING EVERY BEE IN THE GAME (Boss Fight!) - Roblox Bee Swarm Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=AMBxoCy4geI
    * (CODES) WATER COOLED MOWER + REBIRTH GLITCH! - Roblox Farming Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=N2qz2ku_PXY
    * DEFEATING \*SECRET\* QUEEN ANT (ALL UPDATES) - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=gnM1YJRNjPY
    * DEFEATING THE \*NEW\* OLD GODS!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=19gRjVdO64Q
    * I GLITCHED UNDERNEATH THE MAP!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=UKIw6a3G7Fo
    * BOOGA BOOGA 2?!! (New Game)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Qp5TKbVCuHw
    * WHOLE TRIBE VS ONE PLAYER!! (INSANE BATTLE) - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=TAhasqu2xWQ
    * WHICH IS THE BEST SHOVEL??! (Black Hole Vs Golden Nuke) - Roblox Treasure Hunt Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=X9P42mXwkWU
    * THE TOP OF THE INFINITE TOWER!! - Roblox Rocket Simulator (Octagon Tower)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Rri5RlG01Pc
    * WHAT HAPPENS WHEN YOU DROP 2000 GOLD?!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=DIRQZwukF2M
    * DISCOVERING A SECRET ROBLOX TEMPLE! - Roblox Ro-Trip
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=p-5bMBSbS0Q
    * BATTLING TOP PLAYERS FOR \*INSANE\* GEAR!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=I0XLtm6RgEE
    * INSANE BOAT RACE FOR MAGNETITE GEAR!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=au1L_XGEsq0
    * BEST PLAYER GAVE ME INFINITE BAG!! (God Bag) - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=DYYMKIaL5os
    * FLYING RAFT BATTLE W/ LEVEL 1000 (Roblox Booga Booga) - Magnetite Armour Fight
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=BqwArubMSDI
    * MAGNETITE FLOATING ISLAND + DARK TOTEM GOD!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=WC5mRSTjf8Y
    * \*WORLD RECORD\* BIGGEST BASE!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Ew3LC44mzLw
    * WORLDS RICHEST PLAYER DROPS EVERYTHING!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=t5SHS2dijUI
    * GIVING NOOBS OP GEAR!! (CRYSTAL GIVEAWAY!) - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=gTdzIJcNO2I
    * SECRET FLYING GLITCH (How to Fly!) - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=_mNt3Br8zOg
    * RAIDING THE \*NEW\* CRYSTAL ISLAND FOR GEAR!!- Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=8fUGwHZxxa8
    * UNLOCKING THE GOLDEN NUKE AND MAKING BILLIONS!! - Roblox Treasure Hunt Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=AtraXwkQC7s
    * RAIDING TRIBES WITH THE WORLDS BEST PLAYER!! - Roblox Booga Booga
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=Oqk6b1_-hPM
    * \*SECRET\* NEW ANCIENT TREE (FLOATING SUN ISLAND) - Booga Booga Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=RobbehuhrkQ
    * RIDING SHARKS AND WAR MAMOUTH + INSANE TRIBE WAR!! - Booga Booga Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=JDTd256NsK4
    * EASIEST WAY TO GO FROM NOOB TO PRO IN ROBLOX BOOGA BOOGA!
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=P-qdV3zxt0o
    * SECRET SPIRIT KEY FROM \*NEW\* FROZEN GIANT BOSS - Booga Booga Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=jX-P6nEP0Q8
    * (CODES) ALL NEW CODES, ITEMS AND SKINS!! - Roblox Mining Simulator (Beta)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=nHWUk5LZQEw
    * DEFEATING ALL OLD GOD BOSSES (LOCATIONS!) - Booga Booga Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=uyUAwkzkVhA
    * 3x GODLY ESSENCE CHEST UNBOXING!! - Booga Booga Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=f7ZOr_SNDJ8
    * FINDING A GAME BREAKING GLITCH (UNLIMITED STONE!) - Roblox Mining Simulator (Beta)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=HkqOAnvWZzM
    * ALL MINING SIMULATOR CODES!! (Beta) - Mining Simulator Roblox \*2018\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=F198SQLx0SY
    * HOW OP IS THE ZEUS'S STAFF?! (1 MILLION STONE) - Roblox Mining Simulator Beta
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=w_QxXCHfiao
    * OPENING 3 ROYAL CHESTS (LEGENDARIES!) - Bloc Blitz Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=emQQGdDPduA
    * SECRET GOLD TREE + NEW PETS! - Woodcutting Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=FWNLMHDcU8M
    * RAIDING AND DESTROYING MASSIVE ISLAND! - Roblox Pirate Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=qcj_Pqdi8tE
    * THE FASTEST REBIRTH EVER! (WORLD RECORD!) - Treasure Hunt Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=vMs8oQwz2yA
    * UNLOCKING THE BLACK PEARL + MYSTERY ISLAND - Pirate Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=NK73DH_iwPA
    * TRACTOR VS MOTORBIKE! (WHICH IS BETTER?) (CODE!) - Roblox Woodcutting Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=1zA1hGtzesw
    * HOW OP IS THE TRACTOR, BEAM GUN AND QUANTUM BACKPACK!!? - WoodCutting Simulator Roblox (Codes)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=F63SLF7LPiU
    * EVERY CODE FOR WOODCUTTING SIMULATOR - Roblox 2018 \*Working Codes\*
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=HsGoVvJ9PnQ
    * PRO HELPS NOOB WITH THE NUKE!!- Treasure Hunt Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=diuWNweSupw
    * HACKER DELETES THE LEADERBOARD!! - Treasure Hunt Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=RwjYAXAKnOg
    * UNLOCKING ALL NEW SHOVELS AND ITEMS (Nuke) - Treasure Hunt Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ZO1wJmfhL5k
    * NEW MYSTICAL SAND AND CHESTS + NUKE SHOVEL!! - Treasure Hunt Simulator Roblox (Update)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=bhNJaq-lxDc
    * UNLOCKING INSANE NEW SNOW CATAPULT!!- Snow Ball Fight Simulator (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=79AbFoKnJis
    * KID BREAKS MOUSE OVER ROBLOX OBBY (RAGE!) - Roblox Shadow Run
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=xK2NaPJ8uEw
    * DIGGING TO THE BOTTOM OF THE MAP!? (DEEPEST HOLE!) - Treasure Hunt Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=qtloQT2EsNI
    * INSANE NEW DRAGON KING BOSS!! - Zombie Attack Roblox Gameplay
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=E2WSXq3sXDI
    * SELLING 2 MILLION SAND + GOLDEN SHOVEL!! - Treasure Hunt Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=lhEoFS8ycO8
    * UNLOCKING ALL NEW SHOVELS AND ITEMS!! - Treasure Hunt Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=fdeDWxh9Bo0
    * MASSIVE ROBLOX GAME SCAMMED ME?! - Infection Inc Roblox (Tycoon)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=4Mp4Z3XzOgI
    * BUYING METAL DETECTOR + BEDROCK SAND!! - Treasure Hunt Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=EeJlDpt66Zg
    * THE BEST SHOVEL IN THE GAME \*DYNAMITE SHOVEL\* - Treasure Hunt Simulator (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=75-7o39PZ-A
    * ALL WORKING TREASURE HUNT SIMULATOR CODES! (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=LiH8RND_8Aw
    * INSANE INFINITE SNOWBALL GLITCH!! - Snow Ball Fighting/Shovelling Simulator (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=qnjyNud3lgU
    * BUYING EVERY ITEM IN THE GAME!! - Snow Ball Fighting Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=0LZLoxcQj9c
    * BUYING MACHINE GUN SNOW CANNON!! - Snow Ball Fighting Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=AoJ5283xRVQ
    * THIS ZOMBIE GIVES YOU 10,000 XP!! - Zombie Attack Roblox Gameplay (Level Up Tactic)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=RRfjxQiMZIk
    * THE SECRET GLITCHED NOOB TACTIC??! - Cash Grab Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=g0AYadmV3ic
    * WE FINALLY GET TO WAVE 100??! - Zombie Attack Roblox Gameplay (Updates)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=WJIj41JvL6w
    * SUPER OP NEW DOMINUS PET!! (Update) - Roblox Zombie Attack
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=x2rg4s7Zq4Y
    * LEVEL 2000 + $250,000 SPENDING!!- Zombie Attack Roblox Gameplay
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=C89NR6pC_U4
    * 1,000 ZOMBIES VS 1 KNIFE!! (Knife Only Challenge) - Zombie Attack Roblox Gameplay
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=KcVm1d_ANdI
    * I FINALLY HIT IT!! (Level 1,000) - Zombie Attack Roblox Gameplay
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=6gOTtHqjAk4
    * LEVEL 1,000 IN TWO DAYS?!! | Zombie Attack Roblox Game (Demon Overlord Battle)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=bQ1Zilz8bT0
    * LEVEL 500 AND $100,000+ SPENDING SPREE!! - Zombie Attack Roblox Game
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=EVMR6Qr2HOc
    * OVER LEVEL 100 IN ONE DAY! | Zombie Attack Roblox Game (Predator Zombie)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=kvgByKVxoc8
    * THE DUMBEST ROBLOX WIKI QUESTIONS
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=SuECd7gjOWE
    * ANOTHER ROBLOX RIPOFF?! (Brick Planet Review)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=kDUmJAawq_8
    * ROBLOX WEBSITE IN 2006... (Jailbreak, Phantom Forces, MeepCity)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=fJYuK5CPnm8
    * Tazok - The King Of Roblox Clickbait \[Exposed\]
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=x1D_45nz83s
    * MY BIGGEST ROBLOX KILLSTREAK! | Bullet Hell Roblox (Funny Moments)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=nmjBtZNVBn0
    * EVIL ROBLOX DUCK KILLS EVERYONE! (Duck Dash Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=M8iGK9uWwng
    * ROBLOX SPEED HACK \*1k SPEED IN 10 SECONDS!\* - Sprinting Simulator 5 Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=rOz-CstlUNM
    * WE WERE SO FAT WE BROKE THE GAME! | Super Fat Simulator 2 Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=UUwsboIbjew
    * INSANE 15,000 POWER (BEST ON SERVER!) - Wrestling Simulator Roblox (New Game)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=ahyg_uEKZSk
    * INSANE INSTA KILL KNIFE ABILITY \*HACK?\*  - Winter simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=3QB-YwDE2Qc
    * GETTING OVER 6,000,000 POWER!!! (LARGEST ON THE SERVER!!) | Roblox Ghost Simulator (Angels)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=-WYKu_rSi7g
    * MINING MILLIONS OF ROBUX GOLD!! - Roblox Gold Venture
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=oZ2RMzpeZsM
    * JUMPING PAST THE MAP INTO SPACE!! - Ultra Jumping Simulator 2 (Roblox)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=1voII_nLQhM
    * INVISIBILITY TROLLING PATIENTS!! \*HOW?\* - Hospital Life 2 Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=-DeTn6L7FlA
    * IM ALREADY ON THE LEADER BOARDS!! - Roblox Bloc Blitz (Beta + Glitches)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=UQ6BjE9Hc6w
    * THIS HAPPENS WHEN YOU BEAT THE GAME!? - Ultra Jump Simulator Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=aB_txwya3aM
    * IM THE MOST POWERFUL NINJA! - Roblox Saiyan Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=9BTtdcgE04I
    * \*GLITCHED THORS HAMMER!\* - ROBLOX GUN SIMULATOR!
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=EwhSfx8hzjM
    * I FOUND A GAME BREAKING GLITCH!! - Roblox Sprinting Simulator 3 (Christmas)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=WStFA7dsfYE
    * SPEED RUNNING A TYCOON (WORLD RECORD?) - BattleShip Tycoon Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=pLm6JAVBWxs
    * TROLLING AND BANNING ODERS!! (Royale High School Beta) - Roblox
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=F07Jl-jPlcg
    * HITTING THE MOST INSANE TRICKSHOT! \*HACKS?\* - Roblox Knife Simulator
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=qAHhCsIB6tw
    * THE FASTEST SWIMMER IN ROBLOX!! - Roblox Swimming Simulator (Funny Moments)
      * Description references Robux giveaways in the Roblox group.
      * URL: https://www.youtube.com/watch?v=nO7J7m7NpBY
* Poke (Pokediger1)
  * Other
    * SPIN THE ROBLOX DEATH WHEEL (HE LOST 25,000 ROBUX)
      * Video is about a wheel that has a chance to win or spend Robux.
      * URL: https://www.youtube.com/watch?v=3vQT9KP6EeE
  * Phishing
    * HACKING AND GIVING A FAN FREE ROBUX ON ROBLOX!!
      * URL: https://www.youtube.com/watch?v=dQEnfrcMALQ
  * Non-Giftcard Robux Giveaways
    * IMPOSSIBLE HIDE AND SEEK WITH MORPH COMMAND!  (Roblox)
      * URL: https://www.youtube.com/watch?v=a80GNiucA_c
    * I Trapped A POKE HATER In My \*SECRET\* ESCAPE ROOM! (Roblox)
      * URL: https://www.youtube.com/watch?v=h9BR34DoLzQ
    * IF THEY CHOOSE THE RIGHT CUP, THEY WIN FREE ROBUX! (Roblox)
      * URL: https://www.youtube.com/watch?v=zO9TU0qdcIo
    * BELIEVE I'M POKE, WIN 1000 ROBUX! (Roblox)
      * URL: https://www.youtube.com/watch?v=R1_7Zp5RN2A
* Rainway (UseCode_Rainway)
  * Information Collection
    * \*NEW UPDATE\* ADOPT A POLICE DOG! \*TREATS\* (ROBLOX MAD CITY)
      * Contains an ad for the information collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=V8FOj8B0IvA
    * SKY ROPE TROLLING \#2 (ROBLOX BOOGA BOOGA)
      * Contains an ad for the information collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=BXKhrVaUbDQ
    * BOOGA BOOGA BAN UPDATE
      * Contains an ad for the information collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=BoV9gT_qxdc
    * BOOGA BOOGA FINALLY GETS UPDATES! (ROBLOX)
      * Contains an ad for the information collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=r4T2SaUnvqk
    * NEW ESCAPE! TRAIN STATION! JAILBREAK UPDATE! (ROBLOX)
      * Description references a private video about getting free Robux.
      * URL: https://www.youtube.com/watch?v=cfEXRXI2zrk
  * Other
    * HOW TO BEAT FLOOD ESCAPE 2! (NEW ROBLOX GAME)
      * Video includes  "s\*\*\*" (4x),"f\*\*\*" (2x), and "a\*\*" (1x).
      * URL: https://www.youtube.com/watch?v=JP9_ofM_reI
    * HOW TO BECOME A PIRATE ON ROBLOX!
      * Video includes  "f\*\*\*" (59x),"s\*\*\*" (12x), and "b\*\*\*\*" (4x), "a\*\*" (2x), "r\*\*\*" (4x), and "c\*\*\*" (1x).
      * URL: https://www.youtube.com/watch?v=CNqOrpkX1xU
* RazorFishGaming (UseCodeRAZORFISH and RazorFishPengiYT)
  * Information Collection
    * ALL 2 NEW SUPERHERO SIMULATOR CODES - New Game Release/ Roblox
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=w6ExgoXJxto
    * ALL NEW DRILLING SIMULATOR CODES - New Release/ Unique Pets & Ore | Roblox
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=gBjKNNrIECw
    * HOW TO CRAFT A KNIFE In Mad City Roblox
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=AFlAi6kUGb4
    * NEW FAME SIMULATOR CODES Roblox (Update 1)
      * Description references the data collection website gainblox.com.
      * URL: https://www.youtube.com/watch?v=CRjUtFMHGE4
    * Roblox Jailbreak - NEW DINOSAUR MUSEUM ROBBERY! NEW UPDATE OUT! | New Car,  Museum & More!
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=BUFZAMYACdQ
    * Jailbreak DINOSAUR MUSEUM UPDATE RELEASE DATE (Roblox Jailbreak)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=CXYrlxO-rFo
    * \*NEW\* How To Rob The MUSEUM! \[FULL WALK-THROUGH\]  Roblox Jailbreak Museum Update
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=wORbD2ARvEU
    * HOW TO GET TONS OF ROBUX FOR CHEAP
      * Description references the data collection website bloxmarket.com.
      * URL: https://www.youtube.com/watch?v=-TCZhPO6Z0w
    * GET FREE ROBUX FAST & EASY \*NOT CLICKBAIT\*
      * Description references the data collection website earnrobux.today.
      * URL: https://www.youtube.com/watch?v=jYwQYt1eGK4
  * Non-Giftcard Robux Giveaways
    * ALL 14 NEW VACUUM SIMULATOR CODES - New Vacuum/ Update 3 | Roblox
      * Description references a Robux giveaway on Twitter.
      * URL: https://www.youtube.com/watch?v=augy23ui3WE
    * ROBLOX REMOVES LIMITEDS FOR RTHRO LTD!?
      * Description references a Robux giveaway on Twitter.
      * URL: https://www.youtube.com/watch?v=yEDwbsr4ii8
* Realistic Gaming (ImTheRealRG)
  * Information Collection
    * HOW TO GET FREE ROBUX in 2018! APP THAT GIVES YOU ROBUX FOR PLAYING GAMES! FREE ROBUX 2018
      * Description references a download for a data collection mobile app.
      * URL: https://www.youtube.com/watch?v=ESjjCV9WXW8
* realrosesarered (realroses)
  * Phishing
    * I "HACKED" A FAN'S ACCOUNT ON ROBLOX AND TROLLED HER FRIENDS! | Roblox Funny Moments
      * URL: https://www.youtube.com/watch?v=IWSiRNUeBlU
  * Non-Giftcard Robux Giveaways
    * 10,000 ROBUX GIVEAWAY CONTEST! WATCH TO FIND OUT HOW TO WIN! Robux Giveaway In Roblox
      * URL: https://www.youtube.com/watch?v=HqTKlVCJDFs
* Roblox Minigunner (skoonks)
  * Other
    * ROBLOX Exploiters In A Nutshell
      * Uses the "F word" (4x), "S word" (3x), and "P word"
      * URL: https://www.youtube.com/watch?v=APz3TSj0NbU
    * How To Be a ROBLOX Hacker
      * Uses the "F word" (3x) and "S word"
      * URL: https://www.youtube.com/watch?v=KL8vtXmwrhs
    * Roblox "Hackers"
      * Uses the "F word" (16x) and "B word"
      * URL: https://www.youtube.com/watch?v=VIQ73nJgN-I
    * HOW TO GET FREE ROBUX \[LEGIT\] \[WORKS!\] - urban's parody xd
      * Uses the "F word", "S word" (2x), and "B word"
      * URL: https://www.youtube.com/watch?v=00DC1dHwHZM
    * TROLLING A SCAMMER ON ROBLOX - WE EXPLOITED HIM
      * Video contains a lot of explicit text.
      * URL: https://www.youtube.com/watch?v=PwJ7jKxJ3Tc
* ROBLOXMuff (Intervin)
  * Information Collection
    * Poor To Rich 2 | Roblox A Sad Bloxburg Movie
      * Description references the data collection website rbxtoys.com.
      * URL: https://www.youtube.com/watch?v=CLFAnszu3nk
* RODNY (RODNY_ROBLOX)
  * Information Collection
    * ESCAPANDO de la PRISIÓN de ROBLOX!!! 😂 | RODNY ROBLOX
      * Description references the data collection website rbxheaven.com.
      * URL: https://www.youtube.com/watch?v=U89SyfI3ycI
    * LOS MÉDICOS LLEGAN A JAILBREAK!!! ❌😱  | ROBLOX |
      * Description references the data collection website rbxheaven.com.
      * URL: https://www.youtube.com/watch?v=hike8RDe61c
    * TODOS se VUELVEN \*AMON_40L\* en MeepCity!!!💀 | Roblox |
      * Description references the data collection website fastbux.io.
      * URL: https://www.youtube.com/watch?v=Mrers56YGwg
    * ME VUELVO UN \*SUPER HÉROE\* EN ROBLOX!!!💀
      * Description references the data collection website rbxheaven.com.
      * URL: https://www.youtube.com/watch?v=plsvkHwqiVg
    * ME DISFRAZO COMO GOKU EN JAILBREAK!!! 😱
      * Description references the data collection website rbxpacks.com.
      * URL: https://www.youtube.com/watch?v=PMN4eGpjeCM
    * EL JUGUETE MAS EXTRAÑO DE ROBLOX!!! 😰
      * Description references the data collection website rbxpacks.com.
      * URL: https://www.youtube.com/watch?v=zt0ScIZnvCc
    * ESTE JUGADOR DE ROBLOX ESCONDE UN OSCURO SECRETO!!! 😰
      * Description references the data collection website rbxpacks.com.
      * URL: https://www.youtube.com/watch?v=W07eoBfom_A
    * PERDÍ MI ROPA EN ROBLOX!!! 😰
      * Description references the data collection website rbxpacks.com.
      * URL: https://www.youtube.com/watch?v=87dFvsvsW2E
    * COMPRANDO TRAJES DE ROBLOX POR $5 DOLARES!!! 😍 \*hermoso\*
      * Description references the data collection website rbxpacks.com.
      * URL: https://www.youtube.com/watch?v=LqNjEJ8MkS4
    * GANA ROBUX GRACIAS A TUS AMIGOS! TRUCO 2017 ❌(YA NO FUNCIONA)❌
      * Contains an ad for the information collection website rbx.gifts.
      * URL: https://www.youtube.com/watch?v=nNhShLGyiYA
    * COMO TENER 20 ROBUX GRATIS! (YA NO FUNCIONA)
      * Video is about the data collection website rbxrich.com.
      * URL: https://www.youtube.com/watch?v=w3TGHToK2pI
    * ESTA PAGINA Te REGALA15 ROBUX 2017!!! ❌ YA NO FUNCIONA ❌
      * Contains an ad for the information collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=p2hijWLhzOw
    * CÓMO OBTENER +100 ROBUX! 2017 ❌(YA NO FUNCIONA)❌
      * Contains an ad for the information collection website rbx.gifts.
      * URL: https://www.youtube.com/watch?v=coICmU-DxpM
    * GANA 15 ROBUX GRATIS 2017!!! ❌(YA NO FUNCIONA)
      * Contains an ad for the information collection website rbxpoints.com.
      * URL: https://www.youtube.com/watch?v=39aNz1kZwEo
    * COMO TENER TUS PRIMEROS 30 ROBUX!!! 2017 ❌ (YA NO FUNCIONA)
      * Video is about the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=TuasoFIzwkw
* Rovi23 (byRovi23 and MelLovesBunnys)
  * Other
    * Roblox CORONAVIRUS Simulator...
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=Axa-VTK3src
* RussoPlays (RussoTalks)
  * Non-Giftcard Robux Giveaways
    * THE WINNERS OF THE 50k ROBUX GIVEAWAY ARE.... (10 WINNERS)
      * Video is about giving away Robux using group funds.
      * URL: https://www.youtube.com/watch?v=-dT2QhiXCeM
    * HUGE 50,000 ROBUX GIVEAWAY \*HOW TO ENTER\*
      * Video is about entering a Robux giveaway using group funds.
      * URL: https://www.youtube.com/watch?v=92-bOP8egXE
* SeeDeng (SeeDank)
  * Phishing
    * PLAYING ON A FAN'S ACCOUNT IN ROBLOX (SPENDING ALL THEIR ROBUX)
      * Logs into the account of a fan. Mentions a lot of other people sent their username and passwords.
      * URL: https://www.youtube.com/watch?v=9qOAqB6E-z8
* Seniac (MrSeniac)
  * Other
    * ROBLOX SNEEZING SIMULATOR
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=dtQF432tcyM
    * HOW TO GET FREE ROBUX!
      * Description references the "black market for limiteds" and information collection website rbx.place.
      * URL: https://www.youtube.com/watch?v=EakQDIc9ZLM
  * Non-Giftcard Robux Giveaways
    * $10,000 ROBUX GIVEAWAY
      * Robux giveaway uses a Roblox group using group funds.
      * URL: https://www.youtube.com/watch?v=76Sg5bVi1Jo
    * $6000 ROBUX GIVEAWAY
      * Robux giveaway uses a Roblox group using group funds.
      * URL: https://www.youtube.com/watch?v=EmAlMrw6e8A
    * $3000 ROBUX GIVEAWAY
      * Robux giveaway uses a Roblox group using group funds.
      * URL: https://www.youtube.com/watch?v=L7EcvzuKjJw
    * $1500 ROBUX GIVEAWAY
      * Robux giveaway uses a Roblox group using group funds.
      * URL: https://www.youtube.com/watch?v=AGj4BbtDy6k
* SharkBlox (SharkBL0X)
  * Information Collection
    * This Is How You Get Robux For Free!
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=DCsV0c7K5qE
* Snug Life (YT_SnugLife)
  * Other
    * I ACCIDENTALLY DELETED THE NEW \*MYTHIC\* PURPLE SPARKLE TIME FEDORA IN SNEEZING SIMULATORS NEW UPDATE
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=yRRwd3t7ZRw
    * HOW TO GET THE \*SECRET HAT\* IN SNEEZING SIMULATOR! BEST HAT IN-GAME! ABSOLUTELY DISGUSTING STATS!
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=qM-vMFgWPBk
    * SNEEZING SIMULATOR NEW UPDATE! I UNBOXED THE BEST LEGENDARY HAT IN-GAME! \*SUPER OP\* EASY REBIRTHS!
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=kVDf7wX-e94
    * YOU CAN NOW TRAVEL IN \*SPACE\* IN SNEEZING SIMULATOR?! NEW EVOLUTIONS! NEW SPACESHIP?! NEW CODES!
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=BIoWma7yQPQ
    * SNEEZING SIMULATOR ADDED A \*REBIRTH SHOP!\* YOU CAN NOW REBIRTH AND \*UNLOCK\* PERM BOOSTS!
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=yMOXbkwQ-TY
    * I SPENT 50,000,000 MILLION POINTS ON THE \*NEW\* ANCIENT PHARAOH IN ANCIENT RUINS! SNEEZING SIMULATOR!
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=SeDqmaXmspU
    * \*NEW\* SNEEZING SIMULATOR! HOW TO INFECT PEOPLE \*SUPER FAST!\* AND GAIN FAST POINTS!!!!\*KING NOOB OP\*
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=NmJMdWhG9-8
* SrtaLuly (SrtaLuly03)
  * Information Collection
    * BALDI o GRANNY? QUÉ PREFIERES? en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=YNk_9NO-P_o
    * BEBÉ LULY ESCAPA DE LA BESTIA SIN ROPA!! HACKEA Y HUYE en ROBLOX (Flee The Facility) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=jGkKsfAeatg
    * LE PEGAN UNA PALIZA POR QUERER SER SU AMIGO!! HISTORIA de BULLYING MUY TRISTE en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=1BZyRQmHeig
    * TROLEANDO A LA CHICA DEL VESTIDO ROJO de ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=4OsqWJBpsgE
    * NO PUEDES SALTAR, NUNCA!✔️ RETO \*MUY DIFÍCIL\* FLEE THE FACILITY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=IYg_S3KNzIM
    * CUERPO DEFORME!! LOS PERSONAJES MÁS BUGEADOS de ROBLOX  😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=Pp0v34Di3tY
    * LOS DIBUJOS MÁS PERVERTID0S de ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=Oq00NcnsAlU
    * LA BEBÉ MÁS MALVADA e INSOPORTABLE de ROBLOX (BEBÉ LULY en ADOPT ME!) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=oR15tPya3BA
    * LA BESTIA MÁS MALA!! HACKEA RÁPIDO Y HUYE DE FLEE THE FACILITY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=3x900pZteZg
    * TOBOGÁN DE LAVA CONTRA PIKACHU A 999.999.999 METROS en ROBLOX😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=XzpHZ0ZwSZg
    * NO TE CAIGAS EN LOS OBSTÁCULOS MÁS PELIGROSOS de ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=ND6ptXQYUoI
    * BEBÉ LULY Y DERANK SON MUÑECAS BARBIE en LA MANSIÓN BARBIE de ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=fT4GAxnwkGQ
    * SLENDER ME PERSIGUE!! MI PEOR PESADILLA SE HACE REALIDAD en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=GC8w-CNtV2Y
    * LA BESTIA ME TORTURA!! HACKEA Y HUYE RÁPIDO de FLEE THE FACILITY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=VFHkFysThcE
    * SIGUE HISTORIA DE BULLYING QUE HIZO LLORAR A TODO EL MUNDO en ROBLOX (Parte 2)😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=x0oy9sS_gmw
    * TROLLEANDO A GENTE CON BOMBA EXPLOSIVA!! en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=SyqjPaUrgLk
    * SOLO PUEDES IR POR VENTANAS!✔️ RETO \*MUY DIFÍCIL\* FLEE THE FACILITY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=50JfK163NNU
    * REACCIONANDO A LOS VÍDEOS MUSICALES MÁS DIVERTIDOS 5 de ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=WOgAIH8p6dY
    * BEBÉ LULY Y BEBÉ DERANKITO ARRESTAN A GRANNY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=-JhOPfktorQ
    * LA ASESINA MÁS TRAVIESA APARECE EN LOS BAÑOS!! en ROBLOX (Murder Mystery!) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=l8ebs5-zHLY
    * ROBAMOS LOS BEBÉS DE LA GENTE en ADOPT ME en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=8bfDlstvlL8
    * LA BESTIA MÁS TERRIBLE!! HACKEA Y HUYE RÁPIDO, NO PODRÁS ESCAPAR! en ROBLOX (Flee The Facility)😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=zxHvJERvVZE
    * TROLLEANDO CON SKIN INVISIBLE en ROBLOX (Hide And Seek) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=AcSVrGu1aA0
    * LA BESTIA MÁS CAMPERA!! HACKEA Y HUYE DE ELLA en ROBLOX (FLEE THE FACILITY) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=R84dTy-Ju7U
    * BEBÉ LULY ES LA MEJOR BESTIA de FLEE THE FACILITY!! HACKEA Y HUYE DE ELLA en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=idks8TiNtCM
    * MIS PEORES PESADILLAS SE HACEN REALIDAD en ESTE JUEGO de ROBLOX (JASON VIENE A POR NOSOTROS) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=wC18tlK5zKg
    * NUEVA HISTORIA DE BULLYING QUE HIZO LLORAR A TODO EL MUNDO en ROBLOX (Parte 1)😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=fOxoLiiRgVs
    * LA CHICA MÁS INSOPORTABLE QUE HAY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=BrunwK0Kjyw
    * MI NOVIO Y YO VAMOS A SER PADRES!? RESPONDIENDO PREGUNTAS DE SUSCRIPTORES + CAJA WOOTBOX en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=C8ERDecwXqk
    * SI FALLAN, BAILAS DELANTE DE LA BESTIA!! HACKEA O MUERE en ROBLOX (FLEE THE FACILITY) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=JGWsmyE9-Fg
    * NUESTRO NUEVO PADRE NOS ABANDONA!! en ROBLOX (Adopt me con Bebé Luly) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=FMvmC7qMDTo
    * LA ASESINA MÁS RÁPIDA DE MURDER MYSTERY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=Kz2Ggd0Q6fM
    * COMIENDO CEREBROS!! SOMOS INFECTADOS ZOMBI en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=HAVK0pnwl_g
    * MI NOVIO ES LA BESTIA MÁS PELIGROSA!! HACKEA Y HUYE de FLEE THE FACILITY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=tOIkPh023lE
    * EL CICLO DE LA VIDA(NACER, VIVIR Y MORIR) en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=DR-D9p-Kh8M
    * TROLLEANDO A PERSONAS EN OBBY de ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=U3Sa4vMubVo
    * BEBÉ LULY Y BEBÉ DERANKITO HACKEAN Y HUYEN DE LA BESTIA MÁS TEMIBLE en ROBLOX (Flee The facility) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=r4GD6oSnd1Y
    * LOS PAYASOS ASESINOS APARECEN en JAILBREAK de ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=CYVpcQouT28
    * SIGUE LA HISTORIA DE BULLYING QUE HIZO LLORAR A TODO EL MUNDO en ROBLOX (Parte 2)😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=f6JoC6btAY0
    * TROLLEANDO CON SKIN INVISIBLE en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=Fz8XAqW_CRg
    * REACCIONANDO A LOS VÍDEOS MUSICALES MÁS DIVERTIDOS 4 de ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=1sAai42oxdY
    * NO PUEDES HUIR DE LA BESTIA ✔️ RETO \*MUY DIFÍCIL\* FLEE THE FACILITY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=ezPIwYy3OjQ
    * NADANDO CON TIBURONES PELIGROSOS en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=NdzO3gGLu1g
    * BEBÉ DAME TU COSITA vs BEBÉ GRANNY en ROBLOX (Adopt me!) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=lbYCOdnyH6E
    * NO PUEDES FALLAR O MUERES en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=_q9K7cm1Mvs
    * LA BESTIA SE ENAMORA DE MI! HUYE Y HACKEA RÁPIDO en ROBLOX (Flee The Facility) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=sGycsQsyMqw
    * TROLLEANDO A MI NOVIO CON COMANDOS DE ADMIN en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=AA6JLjel0_U
    * LA HISTORIA DE BULLYING QUE HIZO LLORAR A TODO EL MUNDO en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=kAc9gjgAWg0
    * BEBÉ LULY ES LA BESTIA MÁS TEMIBLE!! HACKEA O MUERE en ROBLOX (Flee The Facility) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=CxUhoqGcMys
    * CAMBIO RADICAL! LULY SE CONVIERTE EN ABUELA GRANNY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=pIZoCdD_j5A
    * VIVIMOS EN LAS PEORES PESADILLAS de ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=IAsfB82aC1c
    * EL TOBOGÁN DE SLIME MÁS GRANDE DEL MUNDO (999.999.999 METROS) en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=yzwMbYtjDv8
    * REACCIONANDO A LOS VIDEOS MUSICALES MÁS GRACIOSOS de ROBLOX (Parte 3!) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=D6sxa9N4Rvk
    * SOLO PUEDES HACKEAR CON UNA PERSONA! ✔️ RETO \*MUY DIFÍCIL\* FLEE THE FACILITY en ROBLOX 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=eI1-rUhVn0c
    * LA HISTORIA DE COMO MORÍ ASESINADA SIN PIEDAD en ROBLOX (Murder Mystery!) 😱
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=2W-_Tx-wcKo
* Straw (STRAWHAT_LUFFi)
  * Non-Giftcard Robux Giveaways
    * Complete The HotWheel Track For R$1,000 Robux Challenge!
      * Video gives away using group funds.
      * URL: https://www.youtube.com/watch?v=iP7bdpWjtkk
* Stronbol Roblox (StronbolYT)
  * Information Collection
    * COMO USAR EMOCIONES EN ROBLOX Y BAILAR CON COMANDOS EN ROBLOX
      * Contains an ad for the information collection website robuxhub.com.
      * URL: https://www.youtube.com/watch?v=iTDbvu-g1J4
    * EN ESTE JUEGO PUEDES USAR LA HEADLESS COMPLETAMENTE GRATIS
      * Contains an ad for the information collection website robuxhub.com.
      * URL: https://www.youtube.com/watch?v=4vIo_qYMR00
    * TOP 5 PACKAGES O CUERPOS DE ROBLOX
      * Contains an ad for "black market" Robux.
      * URL: https://www.youtube.com/watch?v=-1ygRjClnJY
    * ESPECIAL 150K SUSCRIPTORES MI NUEVO RAP
      * Contains an ad for the information collection website ezrewards.com.
      * URL: https://www.youtube.com/watch?v=rdEdLaRpBGU
    * EL PROXIMO EVENTO DE ROBLOX SERA DE STAR WARS HAN SOLO!!! EL NOTICIERO ROBLOXIANO
      * Contains an ad for the information collection website ezrewards.com.
      * URL: https://www.youtube.com/watch?v=RG-HjiPJVzA
    * PROBANDO PAPAS ¿RARAS? DE OTROS PAISES 😱 \*CON MI CARA\*
      * Video is about the informtion collection website ezrewards.today.
      * URL: https://www.youtube.com/watch?v=KI5O1tRgekU
    * SIENDO THANOS POR UN DIA AVENGERS INFINITY WAR EN ROBLOX 😱💎
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=zAll6Rk9_nc
    * EL NUEVO DOMINUS DE PLATA EN ROBLOX 😱 ELRUBIUS HACE UN VIDEO DE ROBLOX 😱 EL NOTICIERO ROBLOXIANO😱
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=_qzV664lkJs
    * 🎵 Roblox "El noob" 🎵 Parodia wake me up - Avicii ft JusepeHD 🎵
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=3t5KXrZGv7Q
    * EL NUEVO HELICOPTERO MILITAR PUEDE CARGAR VEHICULOS JAILBREAK ACTUALIZACION DE ANIVERSARIO
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ECzOV9Slhac
    * ROBLOX LA MALDICION DE EL GUEST 666 ROBLOX EL JUEGO MALDITO CREEPYPASTA
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=j6WTMIh3rBw
    * COMO CONSEGUIR ESTA PECERA Y ESTE SOMBRERO DE DAVEY JONES EN ROBLOX ROPA GRATIS ATLANTIS 2018
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=mbFVHa0N2lk
    * COMO CONSEGUIR LAS HOMBRERAS Y LA TIARA GRATIS ATLANTIS EVENTO ROPA GRATIS ROBLOX!!
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=wUPBRsv1Vv4
    * COMO SER ESTOS SUPER HEROES EN ROBLOX 😱
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=KD-sQ3EtJkQ
    * YOUTUBERS VS ILUMINATI VIDEO REACCION EPICA CON XONNEK \*MUESTRO MI CARA\*
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=gYBSdkTFOtY
    * LOS NUEVOS EH INCREIBLES ITEMS EN LA NUEVA TIENDA DE EL EGGHUNT 😮😱
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=vzqJGQ0fP6A
    * COMO OBTENER GRATIS EL ESCUDO DE EL CAPITAN AMERICA INFINITY WAR ROPA GRATIS ROBLOX
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=rKV2ihsk_Og
    * HABRAN OBJETOS GRATIS DE INFINITY WAR!! NUEVO ESCUDO DE EL CAPITAN AMERICA SERA GRATIS ROBLOX
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=-Q5QUr58XfE
    * ESTOS OBJETOS SERAN GRATIS EN ESTE MES EN UN EVENTO?! FILTRACIONES DE ROBLOX
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=sT2HtS7xJNg
    * COMO CONSEGUIR EL FIFTEAM EGG ROPA GRATIS EGG HUNT 2018 😮😱
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=_4NupOPtqJQ
    * COMO CONSEGUIR ESTAS HERMOSAS ALAS EN ROBLOX 😲😱
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=PGP_qidimd4
    * COMO CONSEGUIR ESTE INCREIBLE HUEVO DE DRAGON ROPA GRATIS EGG HUNT 2018 😲😱
      * Contains an ad for the information collection website ezrewards.com.
      * URL: https://www.youtube.com/watch?v=UQXI4cdMsCk
    * EL PRIMER PASO HACIA EL DOMINUS DE ORO ESTA AQUI EN LUMBER TYCOON😲😱
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=8KhJIY6LYRQ
    * COMO PASAR EL TUTORIAL Y CONSEGUIR EL PRIMER HUEVO DE EL EVENTO  ROPA GRATIS EGGHUNT 2018 😲😱
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=bBbn0iUP1-Y
    * ESTE EGGHUNT TENDRA MAS DE 50 HUEVOS TODOS LOS HUEVOS DE EL EVENTO 😮😱
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=oKyGbyau2qA
    * EL VERDADERO SECRETO DETRAS DE LAS CORONAS DE READY PLAYER ONE! 😮😱
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=5i1U5pFAbFA
    * COMO CONSEGUIR LA LLAVE DE CRISTAL Y LA CORONA DE COBRE DE CRISTAL EN 5 MINUTOS O MENOS 🔑😱
      * Contains an ad for the information collection website ezrewards.com.
      * URL: https://www.youtube.com/watch?v=8fU1HOtJIy4
    * ¿DONDE ESTA LA LLAVE DE CRISTAL? POSIBLES UBICACIONES PARA TENER LA CORONA GRATIS
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=4Ly63UcDqyg
    * POSIBLES UBICACIONES DE LA LLAVE DE CRISTAL DE ROBLOX EVENTO READY PLAYER ONE 😵🔑
      * Contains an ad for the information collection website ezrewards.com.
      * URL: https://www.youtube.com/watch?v=ptsJAFzkH1A
    * LA INCREIBLE CINEMATICA SECRETA ESCONDIDA EN JAILBREAK  😮😱 LOS ALIENS DE JAILBREAK SON GALLINAS?!
      * Contains an ad for the information collection website ezrewards.com.
      * URL: https://www.youtube.com/watch?v=s4ApHNQTo9k
    * ESTE INCREIBLE JUEGO RECAUDO MAS DE 1 MILLON DE ROBUX EN MENOS DE UNA SEMANA 😮😱
      * Contains an ad for the information collection website ezrewards.com.
      * URL: https://www.youtube.com/watch?v=iXWbqQL7SUg
    * CON ESTE VIDEO PODRAS CONSEGUIR ESTE INCREIBLE HAT COMPLETAMENTE GRATIS😱🆓ROPA GRATIS ROBLOX
      * Contains an ad for the information collection website ezrewards.com.
      * URL: https://www.youtube.com/watch?v=08zxydGAqtw
    * 🎅ROBLOX ESTA REGALANDO ESTO GRATIS POR NAVIDAD🎅NO CREERAS LO QUE CONTIENEN🎅
      * Contains an ad for the information collection website rblx.pro.
      * URL: https://www.youtube.com/watch?v=gpleBY_9sik
  * Other
    * ¿POR QUE SOLO PUEDES TENER 200 AMIGOS EN ROBLOX?
      * Contains an ad for "black market" Robux.
      * URL: https://www.youtube.com/watch?v=br8STo2I5tI
    * JAILBREAK ANTIGUO VS JAILBREAK ACTUAL
      * Contains an ad for "black market" Robux.
      * URL: https://www.youtube.com/watch?v=5ktq3_U3EBw
    * 😱 JAILBREAK ES SUPERADO! 😱 NUEVOS ITEMS Y SUPER DESCUENTOS LLEGARON A ROBLOX 😱
      * Contains an ad for "black market" Robux.
      * URL: https://www.youtube.com/watch?v=ulRZrRVobso
    * ¿POR QUE LOS DOMINUS SON TAN CAROS? ROBLOX EN ESPAÑOL
      * Contains an ad for "black market" Robux.
      * URL: https://www.youtube.com/watch?v=HrvPJAvG9Xc
    * VISITO UNIVERSAL STUDIOS EN ROBLOX
      * Contains an ad for "black market" Robux.
      * URL: https://www.youtube.com/watch?v=NCFHjdqoWdk
    * ¿REALMENTE EL ROBLOX ANTIGUO ES MEJOR QUE EL ACTUAL? ROBLOX ANTIGUO VS ROBLOX ACTUAL
      * Contains an ad for "black market" Robux.
      * URL: https://www.youtube.com/watch?v=up1nfE0iC6o
    * ¿POR QUE ESTE EVENTO DURA TANTO? ¿ES UN BUG? ROBLOX EN ESPAÑOL
      * Contains an ad for "black market" Robux.
      * URL: https://www.youtube.com/watch?v=yFKVFCLmEK8
    * 🙀 EL SECRETO EVENTO CANCELADO QUE ROBLOX NO QUIERE QUE CONOZCAS! 🙀
      * Contains an ad for "black market" Robux.
      * URL: https://www.youtube.com/watch?v=S1cS5ygWZmc
    * INCREIBLE TRUCO PARA CAMBIAR EL SONIDO DE MUERTE EN ROBLOX
      * uJmu4pSVOpI
      * URL: https://www.youtube.com/watch?v=uJmu4pSVOpI
  * Non-Giftcard Robux Giveaways
    * SI GANAS MI JUEGO TE GANAS 10,000 ROBUX 🤑💸💵 CONCURSO
      * URL: https://www.youtube.com/watch?v=P14NAZJ-yOE
* Sub 
  * Non-Giftcard Robux Giveaways
    * YOU WON FREE ROBUX!
      * Video is about a Robux giveaway using group funds.
      * URL: https://www.youtube.com/watch?v=aepdLCJIe6I
    * HOW TO GET FREE ROBUX IN ROBLOX!
      * Video is about a Robux giveaway using group funds.
      * URL: https://www.youtube.com/watch?v=zI-GGfr-vTE
* Suliin18YT (Suliin18YTT)
  * Non-Giftcard Robux Giveaways
    * MI PRIMER MASCOTA Y COMPRO LA CASA DE HADAS - ADOPT ME ROBLOX
      * Mentions Robux giveaways through nimo.tv.
      * URL: https://www.youtube.com/watch?v=M5lLteKfC8Y
    * 😎 COMPRO LA NUEVA MANSIÓN DE CELEBRIDADES EN ADOPT ME - ROBLOX
      * Mentions Robux giveaways through nimo.tv.
      * URL: https://www.youtube.com/watch?v=_xHeiSnXJEo
* TeraBrite Games (SabrinaBrite and DJMonopoli)
  * Non-Giftcard Robux Giveaways
    * GOTTA CATCH EM ALL! \#3 | ROBLOX - Pokemon Brick Bronze | TeraBrite Games
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=S4XIsjE7blA
    * 🚙 NEW JAILBREAK ATV UPDATE! | ATVs, DONUT STORE & TIRE POPPING! | ROBLOX JAILBREAK UPDATE
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=ZQzPEVYATEA
    * GOTTA CATCH EM ALL! \#2 | ROBLOX - Pokemon Brick Bronze | TeraBrite Games
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=ldKGLO3-mkY
    * IT'S MY BIRTHDAY! | ROBLOX | TeraBrite Games
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=9v2Y5pJeedc
    * GETTING A MOTORCYCLE! | JAILBREAK ROBLOX | TeraBrite Games
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=XX6MM-8n_LE
    * VIEWER BATTLES! | Pokemon Brick Bronze ROBLOX | TeraBrite Games
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=aJ0CxvQkBwI
    * MOTORCYCLES IN JAILBREAK! | ROBLOX | TeraBrite Games
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=tWrF1SHyJjQ
    * NEW MAP YAY! | Murder Mystery 2 ROBLOX | TeraBrite Games
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=z8J3eihTnts
    * ARCADE UPDATE & GETTING DITTO | Pokemon Brick Bronze
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=kDes9B7s9Lw
    * SHINY MAKUHITA! | Pokemon Brick Bronze | ROBLOX
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=DEkXveVYbos
    * SHINY GEODUDE! | Pokemon Brick Bronze | ROBLOX
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=L_sAE8M3HiQ
    * ROBUX GIVEAWAY! | HUNTING SHINY POKEMON (Pokemon Brick Bronze) | ROBLOX
      * Uses group funds to giveaway Robux.
      * URL: https://www.youtube.com/watch?v=X1yPl2wSYL8
* ThatGuy (ThatGuyTG)
  * Non-Giftcard Robux Giveaways
    * ARREST ME FOR FREE ROBUX! (Roblox Jailbreak) \#8
      * URL: https://www.youtube.com/watch?v=10WGqnb_Nq0
    * ARREST ME FOR FREE ROBUX! (Roblox Jailbreak) \#5
      * Gives away Robux for being arrested in Jailbreak.
      * URL: https://www.youtube.com/watch?v=TogY_-vafSA
    * ARREST ME FOR FREE ROBUX! (Roblox Jailbreak) \#3
      * URL: https://www.youtube.com/watch?v=zsYDcHOBOeg
    * ARREST ME FOR FREE ROBUX! (Roblox Jailbreak) \#2
      * URL: https://www.youtube.com/watch?v=o-gB8UlqGHY
    * \*FREE ROBUX\* ARREST ME FOR FREE ROBUX! (Roblox Jailbreak)
      * Gives away Robux for being arrested in Jailbreak.
      * URL: https://www.youtube.com/watch?v=gekomtzuIoE
* The Monkey (MonkeyVsRoblocks)
  * Phishing
    * HACKING A GIRL IN ROBLOX!! (SPENDING ALL THEIR ROBUX)
      * URL: https://www.youtube.com/watch?v=viNT9Ow-hFk
* TheLaughingUnicorn (kristiexo)
  * Information Collection
    * PINK HOLLYWOOD MANSION! || BLOXBURG HOUSE TOUR
      * Description references the data collection website earnrobux.today.
      * URL: https://www.youtube.com/watch?v=lDeX-dKwNkw
    * \[HALLOWEEN SPECIAL\] MAD HATTER || FAN MUSIC VIDEO
      * Description references the data collection website rbxrich.com.
      * Description references the data collection website earnrobux.today.
      * URL: https://www.youtube.com/watch?v=YnNCdrP25XY
    * KAWAII KITCHEN || ROBLOX STUDIO SPEED BUILD
      * Description references the data collection website rbxrich.com.
      * URL: https://www.youtube.com/watch?v=KJwUsguCRCs
    * IF I OWNED ROBLOX || ROBLOX MACHINIMA
      * Description references the data collection website rbxrich.com.
      * URL: https://www.youtube.com/watch?v=H8eNZuI6HEE
    * SHARK VS. UNICORN || ROBLOX SHARKBITE
      * Description references the data collection website rbxrich.com.
      * URL: https://www.youtube.com/watch?v=HY5NHIXXy84
    * HAUNTED BLOOPERS || ROBLOX
      * Description references the data collection website rbxrich.com.
      * URL: https://www.youtube.com/watch?v=1EqOXRXOrkw
  * Non-Giftcard Robux Giveaways
    * 100K ART/VIDEO CONTEST || 100,000 ROBUX
      * URL: https://www.youtube.com/watch?v=ilMQ9WXxCbY
    * 20K CONTEST || ROBUX REWARDS!
      * URL: https://www.youtube.com/watch?v=ZpZQ6bOAA9w
* TheOfficial Fuzion (TheOfficial_FuzionYT)
  * Information Collection
    * I found another SECRET in the BOSS AREA!! | Build a boat for Treasure ROBLOX
      * Description references the data collection website gainrobux.gg.
      * URL: https://www.youtube.com/watch?v=3yF-6-iXOo0
    * 99% OF PEOPLE MISSED THIS SECRET... | Build a boat for Treasure ROBLOX
      * Description references the data collection website gainrobux.gg.
      * URL: https://www.youtube.com/watch?v=r2vHSDiWfn4
    * HOW TO START THE NEW BOSS!! | Build a boat for Treasure ROBLOX
      * Description references the data collection website gainrobux.gg.
      * URL: https://www.youtube.com/watch?v=ZUjpXxPlkXI
    * UPDATE INCOMING! (plushies, Boss, Secrets & codes) | Build a boat for Treasure ROBLOX
      * Description references the data collection website gainrobux.gg.
      * URL: https://www.youtube.com/watch?v=FBEgC1qlEQo
    * \*NEW\* 5 BEST CODES OF ALL TIME!! (Redeem FAST) | Build a boat for Treasure ROBLOX
      * Description references the data collection website gainrobux.gg.
      * URL: https://www.youtube.com/watch?v=frpjRiypsk0
    * \*HUGE\* SECRET AREA!! (best update) | Build a boat for Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=Susicjaz_FU
    * \*NEW\* REVOLVER GUN UPDATE!! 🔫| Build a boat For Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=p02BA6AUJ2s
    * \*NEW\* EGGS HATCHING CODE!! | Build a boat for Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ky-Y1L2oWkY
    * 4 WORKING \*NEW\* CODES! (April 2019) | Build a boat For Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=0OF9kcfPGSo
    * NEW BLOCK LOCATION! \*HARD TO FIND\* | Build a boat For Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=AUn3fHpQzcg
    * \*NEW\* SECRET UPDATE in Build a boat \*OUT NOW\*
      * Description references the data collection website earnrobux.co.
      * URL: https://www.youtube.com/watch?v=5N0zh9ujyfA
    * HIDDEN SECRET \*FOUND\* (spells out Code!) | Build a boat for Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=BUlAOJZR350
    * THE \#1 BEST CODE EVER! | Build a boat For Treasure ROBLOX
      * Description references the data collection website earnrobux.co.
      * URL: https://www.youtube.com/watch?v=4CyzbIWiMIo
    * \*NEW\* BEST MAGNET GOLD GLITCH! | Build a boat For Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=3rWV5hcFLHk
    * Can You Move the TREASURE CHEST using MAGNETS? | Build a boat For Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=w__bjPD7iCQ
    * \*NEW\* LIMITED TIME CODE! | Build a boat for Treasure ROBLOX
      * Description references the data collection website earnrobux.co.
      * URL: https://www.youtube.com/watch?v=0h7NSH8gdoc
    * The OWNER of Build a boat RESPONDED to my Message...
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=vFAUAJNWYz0
    * \*INSANE\* AUTOMATIC CANNONS💣| Build a boat For Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=pX2hJXEGHCc
    * SCALE TOOL \*REVIEW\* (Is it Worth 9,000 Gold?) | Build a boat For Treasure ROBLOX
      * Description references the data collection website earnrobux.co.
      * URL: https://www.youtube.com/watch?v=nWJsHFYnO3I
    * BLACK HOLE BLOCK in Build a boat! | Roblox
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=F27X__bawfg
    * WHATS INSIDE THE VOLCANO? | Build a boat for Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=QGwSzDCmQhI
    * TRADING UPDATE in Build a boat! (Update Idea)
      * Description references the data collection website earnrobux.co.
      * URL: https://www.youtube.com/watch?v=dgP6XZnWdXc
    * CRAZY SOCCER BALL \*GLITCHES\* | Build a boat For Treasure ROBLOX
      * Description references the data collection website earnrobux.co.
      * URL: https://www.youtube.com/watch?v=8s0Imn8oJio
    * NEW YEARS \*SECRET\* CODE! | Build a boat For Treasure ROBLOX
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=SOZeDiqS_Mk
    * Build a boat UPDATE!! (New Quest & Fireworks & More!) RELEASED
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=dvriTSsotAQ
    * \*NEW\* FIREWORKS in Build a boat 🎆(LEAKED ITEMS)
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=qnmKqh18ILg
    * LAWNMOWER GLITCH in Build a boat!
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=vlrhSjN13iQ
    * NEW \*EXCLUSIVE\* CODE! | Vehicle Simulator ROBLOX
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=bnSEtPejMDE
    * \*NEW\* MONEY GLITCH ($3,000,000+) | Vehicle Simulator ROBLOX
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=XIeHRxfgQdY
    * NEW UPDATE! (Candy Harpoons, Glowing Block, Snow Plane!) | Build a boat For Treasure ROBLOX
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=6NOH7ECGGGY
    * \*HUGE\* GINGERMAN MECH! | Build a boat For Treasure ROBLOX
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=kDitN1JRsXs
    * RAREST \*NEW\* WINTER CODE!🎅 | Build a boat For Treasure ROBLOX
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=mbf50j_fdRY
    * Build a boat \*NEW\* TELEPORT GLITCH!
      * Description references the data collection website earnrobux.co.
      * URL: https://www.youtube.com/watch?v=yGW_Y0RvVMg
    * \*BEST\* way to GRIND Blue Present \*BOSS\*! | Build a boat For Treasure ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=olixPsTG_tg
    * HOW TO GET THE \*NEW\* GREEN GIFT! | Build a Boat for Treasure ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=r26GshuLRYM
    * GingerBread SECRETS! (They Follow you) | Build a boat For treasure ROBLOX
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=C0uu8r3QwPw
    * \*HUGE\* CONTROLLABLE PLANE!✈️ | Build a boat For Treasure ROBLOX
      * Description references the data collection website freebux.club.
      * URL: https://www.youtube.com/watch?v=og9ZTqz4ONQ
    * PLANE UPDATE in Build a boat✈️ (BIGGEST UPDATE EVER!)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=m64UVHMbick
    * NEW BLOCK MOVED! \*LOCATION\* | Build a boat For Treasure ROBLOX
      * Description references the data collection website freebux.club.
      * URL: https://www.youtube.com/watch?v=5lwlNNDpoKE
    * \*NEW\* PET DUPLICATION GLITCH! | Pet simulator ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=IKHsLyWGPOM
    * \*NEW\* BEST CODES! ($900,000) | Vehicle Simulator ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=eA8cZ6HlOBY
    * CUSTOM SWORD GLITCH in Build a boat!
      * Description references the data collection website easyrobux.today.
      * URL: https://www.youtube.com/watch?v=u69mAi4wv2U
    * GETTING 50,000 GOLD in BUILD A BOAT!
      * Description references the data collection website robloxwin.com.
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=JDPfrcSGYy0
    * How To Do The HELICOPTER GLITCH! | Build A Boat For Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=6Tr6JzgH9p0
    * EXPLORING THE SECRET PLACE! | Build A Boat For Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=e47c_QVKZfs
    * HIGHJACK BOATS \*GLITCH\* | Build A Boat For Treasure ROBLOX
      * Description references the "black market for limiteds" rbx.market.
      * URL: https://www.youtube.com/watch?v=fQVhEJJWY2k
    * THIS BOAT WILL BLOW YOUR MIND! | Build A Boat For Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=P6rgmtD2w30
    * \*NEW\* TELEPORTING BLOCK (UPDATE) | Build A Boat For Treasure ROBLOX
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=1Hn6ZxCl-P8
    * INSANE MODDED TRUCK! (IN GAME) | Lumber Tycoon ROBLOX
      * Description references the data collection website rbx.gifts.
      * URL: https://www.youtube.com/watch?v=0Vs6jDxKf68
    * How To Get FREE ROBUX! | Roblox 2017 \*LEGIT\*
      * Description references the data collection website earnrobux.today.
      * URL: https://www.youtube.com/watch?v=FU2BBv-Kpgk
    * INSANE JET SKI | Build A Boat For Treasure ROBLOX
      * Description references the data collection website earnrobux.today.
      * URL: https://www.youtube.com/watch?v=a8AtKMxrXiE
    * MANSION BOAT! (1) | Build A Boat For Treasure Roblox
      * Description references the data collection website rbxrich.com.
      * URL: https://www.youtube.com/watch?v=mOzfWoAE1UE
    * HOW I HAVE 3M ROBUX WORTH OF ITEMS! | Roblox
      * Description references the data collection website rbxrich.com.
      * URL: https://www.youtube.com/watch?v=u2GfiWoLDjw
    * 9,999,999 Feet (BROKE THE GAME) | Roblox Build A Boat For Treasure
      * Description references the data collection website rbxrich.com.
      * URL: https://www.youtube.com/watch?v=HkwxrfsEm1M
  * Other
    * NEW PAINTING CODE! | Lumber Tycoon 2 ROBLOX
      * Description references the "black market for limiteds" and information collection website rbx.place.
      * URL: https://www.youtube.com/watch?v=sfviFcrM7PU
    * 10,000 ROBUX GIVEAWAY (OPEN)
      * Uses a Robux giveaway to skew the ratings of a game on an external platform.
      * URL: https://www.youtube.com/watch?v=Y9Zr70rgyKk
* Tofuu (forstaken)
  * Information Collection
    * WE CRAFTED THE RAREST MYTHIC! (Roblox Assassin)
      * Description references the data collection website rbxtrading.com.
      * URL: https://www.youtube.com/watch?v=ik4v09njtl0
  * Phishing
    * HACKING A FAN ON ROBLOX AND GIVING THEM FREE ROBUX!
      * URL: https://www.youtube.com/watch?v=Qr-olKdTSgA
    * HACKING A FAN'S ROBLOX ACCOUNT! (I SPENT ALL THEIR ROBUX)
      * URL: https://www.youtube.com/watch?v=6_Xb00zC_m4
* VeD\_DeV (VeD\_DeV)
  * Non-Giftcard Robux Giveaways
    * DO NOT MOVE TO WIN ROBUX in Jailbreak! (Roblox)
      * URL: https://www.youtube.com/watch?v=m6ueDABWlE0
    * Last To Leave The Circle Wins Free Robux! (Roblox)
      * URL: https://www.youtube.com/watch?v=a52W2QhS6YA
    * HIDE AND SEEK IN JAILBREAK FOR 1000 ROBUX! (Roblox)
      * URL: https://www.youtube.com/watch?v=Dr1ZAAN-mg8
* xMarcelo (xMarcelo)
  * Information Collection
    * 😱 PRÓXIMOS EVENTOS!! E POSSÍVEIS EVENTOS PATROCINADOS do ROBLOX 😍🎉
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=-nIFo4uMkio
    * FAÇA ESSE NOVO BUG E GANHE MUUITO DINHEIRO no JAILBREAK ROBLOX!! 😱😱
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=QLBn_4eMNRs
    * NOVOS ITENS DO EVENTO UNIVERSE ESTÁ CHEGANDO no ROBLOX!! 😱🎉
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=tGNN3ShtesU
    * COMO GANHAR O (Ant-Man Helmet) é (The Wasp) | EVENTO ROBLOX ANT-MAN \[HOMEM-FORMIGA\]
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=eECvU0zj964
    * NOVO BUG DEIXA OS VEICULOS EM CAMERA LENTA no JAILBREAK | Roblox
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=nVr-cUvkbp0
    * 😱 COMO GANHAR O (Medieval Hood of Mystery) é o PÁSSARO (The Bird Says) | CÓDIGOS GRÁTIS ROBLOX 🎁🎉
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=cHCJuKciRJw
    * ASIMO CONFIRMOU!! E UM MUSEU no JAILBREAK, É NOVA CONSTRUÇÃO! 😱🏰
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=1rHbPaLrKSY
    * ROBLOX MUSICAL 2 - Gordinho do Outfit, Dame Tu Cosita 🔊🎵
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=9Zcr7-Hkoxg
    * NOVO BUG QUE FAZ SEU CARRO VOAR 😱 JAILBREAK ROBLOX, Vou parar de jogar Jailbreak? 😰😭
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=9u7__4Q9fhk
    * NOVO EVENTO PATROCINADO HOMEM-FORMIGA ESTA CHEGANDO no ROBLOX?!! 😱😱
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=iOuUj_b-bgg
    * COMO GANHAR A MOCHILA Jurassic World Backpack DO EVENTO JURASSIC WORLD do ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=2SdoCwSFQY4
    * COMO GANHAR O BONÉ Jurassic World Cap DO EVENTO JURASSIC WORLD do ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=M0J8xCR9IgY
    * COMO CONSEGUIR O ÓCULOS É A CAMISA GRÁTIS DO EVENTO JURASSIC WORLD CÓDIGO GRÁTIS ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=e__EV6CbKak
    * COMO GANHAR O FONE Jurassic World headphones DO EVENTO JURASSIC WORLD do ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=mZJCQdc8OVE
    * OS MELHORES ESCONDERIJOS SECRETOS do JAILBREAK | PARTE 2 Roblox
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=jZMK3BqHG-A
    * TRUQUES PARA FACILITAR SUA VIDA no JAILBREAK | PARTE 3 ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Ibh3NazzW7U
    * 😱 EXPLICANDO PASSO A PASSO COMO GANHAR ROBUX no ROBLOX!! 😱💸 (MELHOR MÉTODO MAIS DE 5000 ROBUX!!)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=0Rfygq0CsBo
    * NOVO EVENTO PATROCINADO QUE ESTA POR VIM no ROBLOX 😱🙀 - Jurassic World EVENTO 🎁🎉
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=kRXmJS9yJ90
    * NOVO BUG QUE NINGUÉM CONHECE DE ATRAVESSAR PAREDE no JAILBREAK ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=xhLI5Wo9eY0
    * COMO GANHAR O CACHORRO (Super Pup) EVENTO HEROES | Super Hero Life ||
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=-H3hWFKa0QI
    * CONFIRMADO PRÓXIMO EVENTO HEROES INCREDIBLES 2, MOSTRANDO TODOS OS ITENS, POSSÍVEIS MAPAS no ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=JPkrcCPzVj8
    * COMO GANHAR A MOCHILA ''Incredibles 2 Backpack'' do EVENTO HEROES do ROBLOX | Swordburst 2
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Moy4OLEi_uQ
    * COMO GANHAR (Incredibles 2 Sunglasses é Incredibles 2 Cap) BONÉ é o OCULOS | Roblox Heroes Evento
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=OKu8wpfIreY
    * COMO CONSEGUIR O ''Galactic Helm'' DO EVENTO HEROES no ROBLOX | Swordburst 2
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=UL0Fpu2BcR8
    * COMO GANHAR O FONE (Incredibles 2 Headphones) DO EVENTO HEROES do ROBLOX | HEROES !
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=iGRi0CwpusQ
    * COMO GANHAR O (Incredibles 2 Badge) DO EVENTO HEROES no ROBLOX | Super Hero LIFE 2
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ud501OV_OJg
    * COMO GANHAR A MASCARA DO EVENTO HEROES (Battle Mask of the Hunt) | HEROES ! (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=z6M8tn1kCo0
    * NOVO BUG DE GANHAR MUITO DINHEIRO no JAILBREAK!! 😱💸 | Roblox
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=AQJpHEEOmJc
    * MELHOR METODO DE GANHAR MUITOS ROBUX QUE FUNCIONA!! 😱😱💸💲
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=E-0HkkE_1v8
    * O ROBLOX VAI DAR 90 NOVOS ITENS GRÁTIS PARA QUEM FIZER ISSO!! 😱😱🎁
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=BtRQ_xBK8l4
    * 😱 NOVO BUG DE ABRIR CAIXAS AUTOMATICAMENTE sem GAMEPASS no Roblox Mining Simulator 💰⛏
      * Description references the data collection website oprewards.com.
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=bKi9r_O1LpA
    * NOVA CAPA GRANDE PREMIO, MAPAS do EVENTO Heroes, NOVA COMPETIÇÃO DO EVENTO Jurassic World (ROBLOX) 😵
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=UrnV0o2geKM
    * 😧 NOVA ATUALIZAÇÃO NO AVATAR ANTHRO, Ele esta CHEGANDO!!? 😱😱 (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=eUwbMgYnRmE
    * COMO CONSEGUIR 1 REBIRTH COM 0 (ZERO) BLOCKS MINED é 10 DOMINUS no Roblox Mining Simulator
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=EyYIn_EI-3k
    * CONSEGUIMOS 500 DOMINUS AVALIADOS EM 600.000 MIL ROBUX no ROBLOX Mining Simulator
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=4_tGws6VCN4
    * TROLLEI MEU AMIGO NO SPEED RUN 4 com a GRAVITY COIL no ROBLOX (FOI APOSTADO!!)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=o0VisMjsSCg
    * NOVO BUG INCRÍVEL DE TELETRANSPORTE no JAILBREAK!! 😱 (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=zroV9Vl_YMs
    * NOVOS ITENS DO EVENTO HEROES!! do ROBLOX, PATROCINADOS é NÃO PATROCINADOS (Jurassic Word é Heroes)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=l0lF23jkGKs
    * AVISO SOBRE O EVENTO? Vendas do MEMORIAL DAY no ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ZfQv_dbepfg
    * DOIS NOVOS BUGS PARA ESCAPAR DA PRISÃO SUPER RÁPIDO no JAILBREAK (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=jl59EONbghw
    * PIOR ATUALIZAÇÃO DE TODAS?! - Mostrando TUDO da nova ATUALIZAÇÃO do JAILBREAK (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=X4x41k_jXKU
    * NOVA RODA, LOCAL PARA ROUBAR é LOCAL PARA FUGIR na ATUALIZAÇÃO do JAILBREAK (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Q-Jft0nzfO4
    * NOVO BUG MUITO ENGRAÇADO!! DEIXA SUA CABEÇA INVISIVEL!! no ROBLOX
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=WgsPz80nDRs
    * CONFIRMADO!! NOVA ROTA DE FUGA NA PRISÃO QUE TERÁ NESSE FIM DE SEMANA no JAILBREAK!! 😱😱 (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=_m4GfhB6ljE
    * CONFIRMADO!! NOVO LOCAL PARA ROUBAR no JAILBREAK!! 😱😱 (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=F0LZceW6ybo
    * COMO GANHAR ITENS DO EVENTO BATTLE ARENA SEM FAZER NADA no ROBLOX, (é como ganhar os itens também)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=0aW-Xjn8gnU
    * COMO GANHAR A COROA (Battle Crown) (Elemental Battlegrounds) (Evento Battle Arena)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=0TiGJSkssCo
    * COMO GANHAR A MOCHILA (Battle Backpack) - (Giant Survival 2) - (Evento Battle Arena)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=9R7PtW3eBNU
    * COMO GANHAR O ITEM UNICORNIO (The Unicorn Mace) (Ultimate Boxing) (Evento Battle Arena)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=B9JfYRTHWeY
    * COMO GANHAR A MOCHILA SOLO (Solo Branded Backpack) (Elemental Battlegrounds) (Evento Battle Arena)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Br2C4wjchbs
    * COMO GANHAR O ITEM (Sabacc Playing Cards) - (Giant Survival 2) - (Evento Battle Arena)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=L7TRNDdbvnU
    * NOVO MÉTODO DE ATRAVESSAR PAREDE QUE ESTA FUNCIONANDO!! (Jailbreak ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=oP73KHOp6oI
    * TRUQUES PARA FACILITAR SUA VIDA no JAILBREAK (Parte 2) (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=wePiUjkD9LI
    * OS MELHORES ESCONDERIJOS SECRETOS do JAILBREAK (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=XW1-UPHXFI8
    * TRÊS NOVOS ITENS PATROCINADOS do EVENTO BATTLE ARENA (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ZXF7CHiSNGY
    * CONFIRMADO!! DOIS MAPAS é AS TELAS FINAIS do EVENTO BATTLE ARENA!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=XR1-Hc7cnug
    * COMO USAR OS ÍTENS DOS EVENTOS ANTES DE SEREM LANÇADOS! (Roblox)!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=C-MHyttGKTA
  * Other
    * QUEBRANDO AS REGRAS DO ROBLOX - (Deu ruim?) 😂
      * Video is about breaking the rules using a second account.
      * URL: https://www.youtube.com/watch?v=8fbnc0KiCSc
* ZacharyZaxor (ZacharyZaxor)
  * Phishing
    * HACKING A FAN'S ROBLOX ACCOUNT! \*TROLLING THEIR FRIEND\*
      * URL: https://www.youtube.com/watch?v=M2ybln9PQEM
* ZephPlayz (StarCodeZephPlayz)
  * Information Collection
    * ROBLOX MAKING 6IX9INE AN ACCOUNT
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=3srXgBKQAO4
    * ROBLOX MAKING VENOM AN ACCOUNT
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=eTvOuDtoGHQ
    * the GHOST took over my POPULAR Roblox game..
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=0Gcy0TWV4Ew
    * Easy Way To Get Robux Without Money! (Roblox)
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=dBUnHb_1kFo
    * MEETING JAILBREAK'S OWNER! OMG! (Roblox)
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=N-NV4DX7aTI
    * Get Robux With This App! (Roblox)
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=5GTA5jLNeEM
    * Robux in Roblox
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=tUlxmfpDIqk
    * KID STEALS ROBUX CARD FROM THE STORE.. (Roblox)
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=_2URW63UWEA
    * HOW TO GET THE SPIDERMAN HEAD IN ROBLOX!
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=2Mgi8LBZEE8
    * DISTURBING FLAMINGO GAME IN ROBLOX
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=t6n6GG2meCw
    * THE GHOST MESSAGED ME 1 YEAR LATER.. (Roblox)
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=zXEs9fQeFgk
    * DanTDM Said THIS About Roblox
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=eb5l7WaM3Jo
    * Why DanTDM Quit Roblox
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=H3id5wKBPX4
    * Get Robux WITHOUT Money (Roblox)
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=7XJCO2JXxyw
    * OMG NEW JAILBREAK UPDATE IN A MINUTE! | Roblox
      * Description references a download for a data collection mobile app.
      * URL: https://www.youtube.com/watch?v=D4gN-HfzNMk
    * ROBLOX IN ROBLOX 2018!
      * Description references a download for a data collection mobile app.
      * URL: https://www.youtube.com/watch?v=Dcit20m1KMk
    * Need Builders Club On Roblox?
      * Description references a download for a data collection mobile app.
      * URL: https://www.youtube.com/watch?v=53F8Wajo8aY
    * BEST WAY TO GET FREE ROBUX! (Roblox)
      * Description links to an app that collects user data.
      * Description references a download for a data collection mobile app.
      * URL: https://www.youtube.com/watch?v=Fi4v7q42irs
    * SECRET HIDDEN ROOM FOUND IN PRISON ISLAND! | Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=wEFvc1pWK1U
    * INVISIBLE ADMIN TROLLING IN PRISON ISLAND! | Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=U1VgdcmoHHA
    * EVERY WAY TO ESCAPE IN PRISON ISLAND! (Roblox)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=mvkizuZMMN4
    * ExplodingTNT In Real Life
      * Description references the information collection website oprewards (link is missing though).
      * URL: https://www.youtube.com/watch?v=t1gm9IQjncM
    * TRICKING COPS INTO THINKING I'M A COP IN JAILBREAK! | Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Ngg0jsCffZE
    * ROBLOX MUSIC VIDEO?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=dSmkLMIs8IQ
    * MAKE YOUR OWN JAILBREAK GAME IN ROBLOX! (How To)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=591PGoLd75s
    * HOW TO MEET DENIS IN ROBLOX
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XwA5hthMdcM
    * ROBLOX TOLD ME TO PLAY THIS GAME!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=eTXXIyAYWsw
    * (New) GET FAST & EASY MONEY IN JAILBREAK! w/Pink Sheep | Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=sNLVkkGwHpw
    * ME VS 100 GUESTS IN ROBLOX
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=c727G1QxcYE
    * ROBLOX MUSIC VIDEO UNDERWATER
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=5msWgUpVjIg
    * SECRET SELF DRIVING FEATURE IN JAILBREAK! | Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=dn6HndSy_jY
    * GETTING STRONG IN ROBLOX (Weight Lifting Simulator 2)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=-rOFC9T37Pw
    * ROBLOX MUSIC VIDEO: THE MOVIE
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=jKNehk9epec
    * hey can I have free robux? (roblox)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=3MbKE7JlBMc
    * THIS IS A ROBLOX WORLD RECORD IN JAILBREAK..
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=R1IwG60i5zg
    * THE SECRET BEHIND THE SATELLITES IN JAILBREAK.. | Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=CCQ-1Bfeq04
    * ROBLOX MUSIC VIDEO \#5
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=iOvO0bxto2A
    * WHAT IS THIS IN THE NEW JAILBREAK RELEASE?! | Roblox (Jailbreak Update)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=RVZ_hakQzZI
    * THIS NEW BUILDING IN JAILBREAK.. | Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=WNE3EGGS3gY
    * BEST ROBLOX MUSIC VIDEO
      * Description references a video about getting Robux for filling out surveys.
      * URL: https://www.youtube.com/watch?v=EZyqSwepvKQ
    * ROBLOX MUSIC VIDEO \#4
      * Description references a video about getting Robux for filling out surveys.
      * URL: https://www.youtube.com/watch?v=cdaqPMhTdQ0
    * ROBLOX MUSIC VIDEO \#3
      * Description references a video about getting Robux for filling out surveys.
      * URL: https://www.youtube.com/watch?v=-nvRDeFRWAA
    * ROBLOX MUSIC VIDEO
      * Description references a video about getting Robux for filling out surveys.
      * URL: https://www.youtube.com/watch?v=QTbRWUhK3wI
  * Other
    * Roblox is HIDING this from you..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=KH0mA4E6li8
    * I Added BUILDERMAN As A Friend.. (Roblox)
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=yXd-8f8MCBA
    * I Went To a SECRET Roblox Meeting..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=LWR05djPi2E
    * ROBLOX is ENDING...........?
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=k4U4bEabCjs
    * I Set Up SECURITY CAMERAS In The ROBLOX Office And What I Found Was Shocking..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=dTe9SQhQmTA
    * Say GOODBYE To The OOF Sound In Roblox..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=FIKmqOcfPrg
    * Builderman Reveals The TRUTH About Roblox..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=eZyanLTxCMc
    * I Joined BUILDERMAN'S ROBLOX Game..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=kmLuAs1ftZU
    * I Joined ROBLOX'S VIP Server..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=BLTmlXJlB_U
    * ROBLOX showed me the secret free catalog..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=Ada4kpvqJ0c
    * I Added ROBLOX As A Friend..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=qUbU1akDnm4
    * ROBLOX Gave Me ADMIN..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=LAKqAIXoENE
    * I Joined ROBLOX'S Game..
      * Video involves impersonating Roblox employee accounts.
      * URL: https://www.youtube.com/watch?v=g_LIiAT7Y6k
    * MAKING JAMES CHARLES a ROBLOX ACCOUNT
      * Includes the words "Sex Tape" at 2:53
      * URL: https://www.youtube.com/watch?v=3P6LVJBFkGg
    * I Said \*\*\*\* For The First Time.. (Roblox)
      * Video includes  "f\*\*\*" (2x), "b\*\*\*\*", and "bulls\*\*\*" included but bleeped out. Middle fingers are also used at the end.
      * URL: https://www.youtube.com/watch?v=t_W8ReEfuHY
  * Phishing
    * I HACKED A FANS ACCOUNT AND GAVE THEM ROBUX! | Roblox
      * URL: https://www.youtube.com/watch?v=z_a7yQ9oVuk
  * Non-Giftcard Robux Giveaways
    * Don't Say ANYTHING To Win 10,000 ROBUX (Roblox)
      * URL: https://www.youtube.com/watch?v=__wCIZWHUOM

## Conclusion
The inconsistent enforcement of the Roblox terms of use and laws in the United States needs to be addressed to improve
community safety and improve the relations of the influencer program and developers. Robux giveaways and phishing can
give legitimacy to malicious people trying to get access to users' accounts while information collection puts the
information of minors at risk. Addressing these will make developers have more confidence in moderation and Roblox's
marketing team.