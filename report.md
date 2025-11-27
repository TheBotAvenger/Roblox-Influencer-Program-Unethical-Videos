# Roblox Influencer Program Unethical Videos
Generated November 27, 2025<br>
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
* Total videos: 1,067,459 videos
* Total videos found that match keywords: 58,461 videos
  * Total unprocessed videos: 12,437 videos
* Total videos found that are processed and marked: 525 videos 
  * Non-Giftcard Robux Giveaways: 356 videos
  * Information Collection: 161 videos
  * Other: 4 videos
  * Phishing: 4 videos

### No Videos Found
The following channels had nothing appear with manual searching. Videos may exist, but were not found.
* 39jeshi (39jeshi)
* 3SB Games (cakemiix and 3SBMiichael)
* Aati Plays (AatiPlays_Official)
* AbsintoJ (AbsintoJYT)
* Acenix (AcenixGatoo)
* AEREN (AaronDRonin)
* AidanGamerHD (AidanGamerHDXD)
* AL3XEY (AL3XEEY)
* Alaska Violet (alaskaviolett)
* Alex Crafted (HeyCrafted)
* Aline Games (AliineGamesYT)
* AlphaGG (0StarCode_AlphaGG)
* AlvinBlox (Alvin_Blox)
* Aly1263 (aly1263)
* Amberry (Amberrry)
* Andre Nicholas (andrhevn21)
* Andshot (Andshotx)
* Angelazz (Angelazz)
* Angeltanked (angelchopped)
* AnielicA (ANIELICA01)
* Anime Revolt (Hashir4553)
* Ant (Cringley)
* Ant Antixx (Ant_Antixx)
* ApplyingTM (ApplyingTM)
* Argo Play (YT_Argo)
* arielle (ariellesyt)
* Armenti (Armenti)
* Ashley (codeeunicorn)
* AshleyBunni (BunniYT)
* AtlasZero (AtlasBlox7)
* Auratix (Auratix)
* Avocado Playz (AvocadoPlayzOfficial)
* Awhxliv (awhxliv)
* Ayzria (Ayzria)
* b3eleyy (b3e_leyy)
* BaconHair Originals (BaconHairGGOP)
* BaixaMemoria (CaueBM)
* BALETKA07 (BALETKA07)
* Bandi (BandiRue)
* Bandites (Bandites)
* Bax (DefinitelyNotBaxtrix)
* Bebe Milo (BebeMiloAmiwito)
* BeeBlox (ThePapaBee, imgabibee, MrBee, and LaMamaBee)
* Benni (ImBenni)
* Benx (Bensonheimer and ebruulein)
* BereghostGames (snapple43, pixiedust423, Bereghost, and valadin1)
* BestrolYT (BestrolYT)
* BETO GAMER (betogamer_01)
* Bia Gamer (BiiiaGamerYT)
* Bianca Nayi (bianqui_nayi123)
* Bibi e Lud (BibibloxYTB and LudBloxYTB)
* Bidinho (bidinh0)
* BigB (BigBst4tz22)
* BigDadT (biggerdadt)
* BIGHEAD (Bighead_StarCode)
* BitSquid (BitSquid)
* Blam Spot (Alopek)
* Blox4Fun (SgGamersDad, GustTv, and SimasGamer)
* BlueBug (TheBugBlue)
* Bluxxy Gaming (BluxxyGaming)
* BolzarX (xxBolzxx)
* Bonnie Builds (BONNlEBUILDS)
* BramP (BramPeeee)
* Brancoala Games (brancoalado, marcossmm, lauroala, and claudiacraudete)
* BREN0RJ (BREN0RJ7)
* Brincando com a Keke (Keke_KerenYT)
* BrittanyPlays (Britt_Blox)
* BRYAN MCQUEEN (IamBryanMcqueen)
* Bubbles (bubblestoxic)
* Builderboy TV (builderboy100009)
* Buur (BuurmanTenus)
* ByDerank (ByDerank_YT)
* Cafazera (CafajesteGOD)
* Calixo (BuBreezy)
* callmehhaley (callmehhaleyx)
* Canal Cahcildis (cahcildis)
* Captain Capi (CaptainCapii)
* Captain Tate (CaptainTate21)
* Captainly (UseStar_CodeCAP)
* Carbon Meister Plays (CarbonMeister)
* Cari - Roblox (Carihyper)
* Carmi Games (carmiYTgames)
* Carol TV (caaroltv)
* CatFer (SoyCatFer)
* CATYPINK PLAY (catypink15)
* cazum8 (cazum8)
* Ceddy (Ceddy)
* Cee\_berry (Cee\_berry)
* Cerso (Cerso93)
* Chef Rae (blacklowie)
* Chekecheto (Chekelete1)
* Cheo Power (cheopower)
* CherryAhrizona (baby_ahrizona)
* Chillz (ChillzSubZero)
* Chizeled (Chizeled_YT)
* Chocoblox (Chocoblox_93)
* Chrisandthemike (chrisatm)
* Christocream Roblox (Christocream)
* Chrisu (ChristsuYT)
* cKev (cKev)
* Claus (ClausOFC)
* Cleansey (CleanseyFamily)
* ComfySunday (ComfySunday)
* Complex Roblox (IamComplexRoblox)
* Conor3D (Conor3D)
* Cookie Cutter (CookieCutter)
* corny (co_rny)
* Cosmic (bluecosmiic)
* CrazyErzy (CrazyErzy7u7)
* Cristi Suki (CristiSukiYT)
* CrystalSims (CrystalSims)
* CubeINC (RealCubeINC)
* Curtified (CurtifiedYT)
* Cute Cookie Gaming (CuteCookieGaming05)
* Daffa super (Daffa_super2)
* DandanPH (DandanPH)
* Daniele Jesden (DannyJesdenYT)
* DanteField (etnad34)
* DaPandaGirl (Da_PandaGirl)
* DatBrian (DatBrian)
* Daylins Funhouse (DaylinsFunhouse, FunHouseDadWasTaken, and Fun_HouseMom)
* DeeterPlays (DeeterPlays)
* DeGoBooM (BumiReal)
* DernD (StarCode_dernd)
* Devo (Devovorya)
* Devoun (DevounTV)
* Dexter Playz (D3XT3R_PLAYZ)
* DfieldMark (OfficialDfield)
* DigitizedPixels (DigitizedPixels)
* Digopollo (DigoPollos)
* Divine (vrcia)
* Diário do Casal Gamer (VictorNuclear and FernandaTreta)
* Dogbon62 (Dogbon62)
* DOLLASTIC PLAYS! (DollasticDreams)
* Dom (DDomss)
* Dr laba (Dr_laba)
* DraconiteDragon (ItsDraconiteDragon)
* dragonplatinum (dragonplatinum)
* DrakeTacos (Brianvid19)
* Drei Diego (AndreiEon124)
* DUDU Betero (DUDUBetero\_ofcYT, DuduBetero, RafaBetero\_BTR, and RafaBetero)
* DV Plays (DVwastaken)
* El Magnum (MagnumStormus)
* elDanielGT (DanielGTReal)
* ElemperadormaxiYT (EmperadorMaxiOFICIAL)
* Elite (E1ite_E)
* Elitelupus (elitelupus)
* Ella (ellasparis)
* EmpireBlox (EmpireBloxOficial)
* ErnieC3 (ErnieC3_YouTube)
* Estela (Estelacoutin)
* EthanGamer (EthanGamerTV)
* ethanolodj (ethanolodj)
* Euri Wings (EuriWings)
* Evanbear1 (Evanbear1)
* EYYCHEEV (EYYCHEEV)
* F (fminusmic)
* FadedPlayz (YTFadedPlayz)
* faeglow (faeglow)
* FakerUp (NotFakerUp)
* Família Betero TV (BianoBetero)
* Fanaticalight (Fanaticalight)
* FancySmash (RealFancySmash)
* faulty (Faulltyy)
* febatista (febatistaYT)
* fer999 (StarCode_fer)
* Fera (Fera)
* FGTeeV (FGTeeV and DrizzMcNizz)
* Fini Juega (FiniJuega)
* FishyBlox (Fishy)
* FlameRBX (SnowRBX_Youtube)
* Flamingo (mrflimflam)
* flashii (flashiilindo)
* Flebsy (Flebsy)
* FloatyZone (FloatyZone)
* Flxral (xoFlxral)
* FlyBorg (KaykBlox)
* Folix (Mrpresident1302)
* Foltyn (TheRealFoltyn)
* Foncri (Foncri)
* Fraser2TheMax (Fraser2TheMax)
* frenchrxses (frenchrxses)
* FUDZ (fudsim)
* FunkySquadHD (UseCode_Funky)
* FunnyBunny (Jxssivca)
* FunPiggy (CelestialPiggy)
* Furious Jumper (furi0us_jumper)
* FusionZGamer (NotFusionZGamer)
* Gaby Gamer (GabyGamerrOficiall)
* Gallant Gaming (GallantGaming)
* Gamer Kawaii (lGamerKawaiiYT)
* Gamer Robot (Zioles)
* GamerHexapod (Hexapod_xD)
* GamerNom (MaybeItsMyFault)
* GamingMermaid (Aquaerria)
* GamingwithVYT (GamingwithVYT)
* GH0Ks (GHOKSZIN)
* GhostInTheCosmos (GhostInTheCosmos)
* GianBlox (GianBloxStarYT)
* Godenot (godenot)
* Godstatus (CallMeGodstatus)
* GoldenGlare (GOLD3NGLARE)
* grace k (yt_graceek)
* Graser Roblox (MasterGraser)
* Gravycatman (GrumpyGravy)
* GroovyDominoes52 (GroovyDominoes52)
* Gunslaya (iGunslaya)
* Hakobe (PHMittensSTARCreator)
* HandN (xHandNx)
* hannnahlovescows (hannnahlovescows)
* HappyBlack (HappyThreePro)
* hatedquis (VURIIN)
* Heorua (Heorua)
* HeyDavi (DAVIPLAY_14)
* heyhana (iheyhana)
* HeyRosalina (HeyRosalina)
* Hoops The Bee (hoopsthebee)
* HW5567 (HW5567)
* Hxyila (hayiIaaa)
* Hyper (DylanTheHyper)
* HyperCookiie (HyperCookiie)
* iAirRon (iAirRon)
* iamSanna (notiamsanna)
* IBella (IBella)
* iBeMaine (ibemaine)
* iBugou (iBugou)
* iDatchy (iDatchy)
* iKotori (iKotori)
* ImaGamerGirl (ImaGamerGirl)
* InceptionTime (InceptionTime)
* Inemafoo (Inemajohn)
* InquisitorMaster (inquisitormaster)
* IntelPlayz (IamRoBuilder)
* It's Akeila (itsakeila)
* It's Siena (ItsNotSiena3)
* iTownGamePlay \*Terror&Diversión\* (iTownGamePlayYT)
* Its Matty (YT_ItsMatty)
* Its Starlight plays YT (Its_starlightplaysYT)
* Its\_CxldKid (Its\_PupBee)
* ItsFunneh (Funnehcake)
* ItsMatrix (MatrixPlaysRB)
* ItzSweaking (ItzSweaking)
* ItzVexo (ItzVexo_STARCODE)
* J-Bug (J_Buggo)
* Jake Globox (JakeGlobox)
* Jake Pudding (JakeZPudding)
* Jameskii (RealJameskii)
* JamesNotTaken (NotJamesRBLX)
* Janet and Kate (KittyJanet and Kate9071)
* JavaCreates (JavaCreates)
* javie12 (javie12)
* JCWK (JCDoesGaming_YT)
* Jeancof (Jeancof)
* JeffBlox (JeeffBlox)
* JehxTp (JehxTp)
* jello queen (JelloQueenYT)
* Jenstine (jenstine and jenstinex)
* Jeny\_Punker (Jeny\_Punker)
* JesuaCunnigham (Jesua_Cunnigham)
* Jie GamingStudio (JieeVideos)
* Joao joao (Joao_joaooficial)
* Joe Albanese (Joey_Albanese)
* JoeyDaPrayer (mrjojoman131)
* JoJocraftHP (JoJocraftHP)
* JonesGotGame (JonesGotGame)
* Juaumzinho (JUAUMZONES)
* Judo Unicorn (judounicorn11)
* Juega y Diviertete con Vicky (Vickycasti_laPro)
* Juicy John (Majojocl)
* Julia MineGirl (Crisminegirl and JuliaMinegirl)
* Julianbank (blvckbank)
* Junell Dominic (Junewuuu)
* JunRoots (cheeseandcakejuice)
* JustHarrison (JustHarrisonYT)
* jvnq (jvnqYT)
* JymbowSlice (JymbowSliceYT)
* Kaden Fumblebottom (jokerkid5898)
* Karim Juega (karimjuega)
* Karola20 (karola20YT)
* KelseyAnna (KelseyAnna)
* Kelvingts (Kelvingts)
* Kepu The Cat (KepuTheCat)
* Khanh Huyen DIY (St4Tick)
* KHORTEX (StarCodeKTX)
* Kindly Keyin (KindlyKeyin)
* King Luffy (KingLuffy_YTsub2me)
* KingOfYouTube (WELIX)
* kitsubee (KlTSUBEE)
* Kitt Gaming (starcode_kitt)
* Kodak (KodakOF)
* KraoESP (KraoESP)
* Kreative Kyle (Kreative_Kyle)
* KreekCraft (KreekCraft)
* Krystin Plays (KrystinPlays)
* KTG Gaming (lifetimeobcman123)
* KunicornCraft (kawaii_kunicorn)
* Kvssidy (kvssidy)
* KyyBlox (ThatGuyTG)
* LAMI (Lami_Roblox)
* Lana's Life (Lanaraee)
* Lani (LaniiPlayzzz)
* LankyBox (LankyBoxGamesAdam)
* Lari Gamer (iiLariGamer)
* Laughability (Laughability)
* Lawlieet 20 (XLAWLIEETX)
* LcLc (LcLcO1)
* Leah Ashe (NotLeah)
* LegendLeo (FoamyBubbIes)
* Legolaz (LegolazYoutube)
* Lenay (lenay_ROBLOX)
* Leshero Morrazo (MoroSamy)
* LiaBlossoms (LiaBlossoms)
* Liege North (LiegeNorth)
* LilahBloxy (3lilahbloxy)
* Lilly BloxTV (Lilly_TVs)
* LimaMosca (LimaMosca)
* Linkmon99 (Linkmon99)
* LinMeiLee (ItzLinPlays)
* LipeBlox (LiipeYoutuber)
* locus (locus200k)
* LOGinHDi (L0GinHDi)
* Lokis (lokis9340)
* Lonnie (GPR3)
* Lord (Lorrd_ofc)
* LordMetalizer (Metalizer)
* LOUD NAYU (LOUDNAYU)
* Lovely Ela (LovelyEla98)
* Lowni ROBLOX (lownione)
* Lowuis (Sircheko_yt)
* Luky (LukyBloxYT)
* LunaPorDos (LunaPorDos00)
* Lunar Eclipse (Lunar3clispe)
* Luuy (iiLuuy)
* luvsunny (sunshrxxm)
* luvxshine (luvxsh1ne3)
* Luzablxx (Lzablxx)
* M4DARA TR (MADARATRTRTR)
* Mackenzie Turner Roblox (MackenzieTurnerYT)
* Magicbus (MagicbusYT)
* Mahdi (MahdiAlNur)
* Mahdi (RealMahadi)
* Maislie (Maislie)
* MakotoUchiha (MakotoYoutube)
* MamyBlox (xMamys)
* Mandinha Game (MandiinhaGame)
* Manucraft (ManucraftYT)
* Mariana Nana (marianavasco)
* MathFacter360 (MathFacter360)
* Matsbxb (MATSbxb)
* mayrushart (mayrushart)
* MedTw (MedTwYT)
* MeEnyu (EnyuZee)
* MeganPlays (TheMeganPlays)
* MelzinhaMel Games (MelzinhaMelGames, Detona\_Anderson, and Princesa\_Alicinha)
* Mia Games (MiaZaffyt)
* MIANNN (MIANNNGAMER)
* MICHI RØBLØX (michineyley)
* MicroGuardian (MicroGuardian)
* Mila FunPlayer (trxmila)
* MiniBloxia (SubToMiniBloxia)
* Model8197 (Modeldog8197)
* MoFlare (MoFlare)
* Moody Unicorn Twin - Roblox (moodyunicorntwin)
* Moonfallx (moonfallx)
* Mr. Bunny (AlanConejito_7u7)
* Mr\_Booshot (Mr\_Booshot)
* MrCrainer (MrCrainer1422)
* MrLokazo86 (Lokazo86)
* Mud (DunkinMud)
* Muneeb (itsMuneeeb)
* MUÑECOB (MunecooB)
* Mxddsie (mxddsie)
* MxghtyJxstin (MxghtyJxstin)
* Myster0y (Myster0y)
* MyUsernamesThis (MyUsernamesThis)
* Nanndo (Nanndo)
* NanoProdigy (NanoProdigy)
* Natasha Panda (NatashaPandaaYT)
* Nathy Super Gamer (Nathy_SuperGamer)
* Natsuki Sel (Natsuki_Sel)
* NavyXFlame (NavyXFlame)
* Nicole Kimmi (Nicole_Kimmi)
* Nicole Maffi (NicoleMaffi and vanessamaffi)
* NightFoxx (NightFoxx)
* Ninjagaming (Ninjagaming)
* NO\_DATA (NO\_DATA)
* Noob Master (N00B_Mast3rXD)
* not here anymore (notBeaPlays)
* NotAmberRoblox (NotAmberRoblox)
* notive (notive)
* O1G (TotallyNotO1G)
* ObliviousHD (ObliviousHD)
* officialnoobie (oofficialnoobie)
* oGVexx (LouDaREALGamer)
* OKEH SQUAD (Rbellomello)
* Olix (Olix)
* OMB Gaming (OMBcreates)
* Ominous Nebula (StarCode_Ominous)
* OneSpottedFriend (OneSpottedFriend)
* ORION (elOrioNYT)
* Oscar (VanillaOreoCat)
* PaanDuh (PPaanDuh)
* PaintingRainbows (RainbowsYT)
* PandaLemonTart (PandaLemonTart)
* PandazPlay (ComplexConniee and ComplexJason)
* PandinhaGame (PandinhaGame)
* Pankeyk San (MrPankeyk)
* Papile (jupapile)
* Paula Urbaez (PaulaUrbaez)
* peachyylexi (Lexiipeach)
* Peepguy (peepguyx)
* PeetahBread (PeetahBread)
* Peter Toys (PeterToys)
* PghLFilms (PGHLego1945)
* Phoeberry (Phoeberry)
* PinkFate Games (PinkFateYT)
* PinkLeaf (RenLeaf)
* Play with me - Apps and Games (da\_dania and kaan\_xy2)
* Plech (SoyPlechango)
* Plique Games (PliqueYT)
* Poke (Pokediger1)
* PolarCub (PolarCub)
* PremiumSalad (premiumsalad)
* PrestonGamez (PrestonPlayz)
* PREZLEY (PrezleyOfficial)
* PRIME FURIOUS (Prime_Furious)
* Princess Royale (IAmPrincessRoyale)
* Princess Tori (ItsToriTimeYT)
* ProbIems (Meelxicano)
* Proder (Prod3r)
* ProjectSupreme (ProjectSupreme)
* ProSidu (prosiduzao)
* qaluo (Qaluuo)
* Quimic (0hRui)
* Raconidas (Raconidas)
* RadioJH Games (audreyradio)
* Rainster (RainsterYT)
* Rainway (UseCode_Rainway)
* Rajo END (Rajo_END)
* Rax (RaxBLX)
* Razor (mprazor)
* RazorFishGaming (UseCodeRAZORFISH and RazorFishPengi)
* Realistic Gaming (Starcode_RealisticG)
* realrosesarered (realroses)
* Rebootedpoppy (CryptedPoppy)
* Red Ninja (STARC0DE_RedNinja)
* REDKILL (RED_YTBE)
* Rektway (Rektway)
* RELLGames (RELLvex)
* Remainings (Remainings)
* RiusPlay (RiusPlayYTYTYT)
* Robin Hood Gamer (RobinHoodGamer10)
* roblox az (robloxazytt)
* Roblox Minigunner (skoonks)
* ROBLOXMuff (Muffinzs)
* Robotz (R_obotz)
* Robrox (IchBinBrox)
* Robstix (Robstix)
* Robuilds (Robuilds)
* Rod Velloso (viddrox)
* RODNY (RODNY_ROBLOX)
* Rovi23 (byRovi23 and MelLovesBunnys)
* roxhi roblox (12345roxyhi12345)
* RoxiCake Gamer (RoxiiiCakeee)
* Royale Dior (RoyaleDior)
* Ruby Games (ruby_rubeYT)
* RufflesOfficial (RufflesPlaysMC)
* Rupthy (Rupthy)
* RussoPlays (RussoTalks)
* S\_Viper (S\_Viper)
* SamHandling (Digito)
* Sant (heysant2018)
* Santino Tossi (SantinoTossi03)
* Saoli (saoli11)
* SATSHA JUEGA (yt_SATSHAJUEGA)
* ScooterSmash0 (ScooterSmasher0)
* ScriptedMatt (ScriptedMatt)
* SDMittens (SDMittens)
* sebee (23Sebee)
* Seer (ReaISeer)
* Seniac (MrSeniac)
* seqshell (seqshell)
* shadow network (realshadownetwork)
* ShanePlays (SGC_Shane)
* SharkBlox (SharkBL0X)
* Shaylo (YTshaylo)
* Shiloh (okgamerman)
* ShisuiXI (Sh1suiXI)
* ShowBlox (IM_Celestial)
* Shrekyou21 (shrekyou21)
* Signicial (Signicial)
* SiimplyBubliie (SiimplyBubliie_YT)
* Silent (RandomYoutuber0202)
* SkippyPlays (skipsk0p)
* Skyrain (iiPietra_GamesYT)
* SlEGHART (SlEGHART)
* Sleigher (Gsleigh)
* Slykage (Sly_Kage)
* Sniffycat (SnickerHoops)
* Snug Life (YT_SnugLife)
* Sofia Tube (sofiatube7)
* Solem Archive (iJackeryz)
* Sons Of Fun (SonsofFun_YT)
* Sonti (Sonti)
* Sopo Squad Gaming (mikedrop937, CammyxBoba, and soposhadow)
* Soy Blue (SoyBluee)
* Soy Mandy Games (SoyMandy2020)
* SoyBlecus (Blecusito)
* SoyLuz (LuzCute24)
* Spekツ (spek)
* Spok (spokchris)
* Squid Magic (Foolzy)
* SrJuancho (JuanchoAcostado)
* SrtaLuly (SrtaLuly03)
* Stanjee (Stanjeeplayz)
* steak (steakwad)
* Stevebloxian (steven111234)
* stream CA Roblox (stream\_CA and Dominus\_CA)
* Striker180x (strikerl80x)
* STUD (s7ud)
* Sugar Roblox (ourfire and ChrisOurFire)
* SulyMazing (iamtheonepoundfish)
* SunnyxMisty (xSunnyxMistyx)
* Sunset Safari (HeySunsetSafari)
* SuperDog Tyler (superdog_tyler)
* SweePee (SweePeeRBLX)
* Só Por Causa (Est3vA0)
* Sören Abbaok (Abbaok)
* Tangochini (Tangochini)
* Tapparay (Tapparay)
* TapWater (tapwat4r)
* teenager paul (teenagerpaul)
* Tekk (tekkyoos)
* Telanthric (Telanthric)
* Temprist (Temprist)
* TeraBrite Games (SabrinaBrite and DJMonopoli)
* Tesla Motors (PatooLocooYT)
* Tex HS (TexWillerHS)
* th3c0nnman (th3c0nnman)
* The Crystalline Gamerz (Crystalaco and Emeraldoft)
* THE KAPOLAR (Kapolar1)
* The Meg and Mo Show (MegaSquadMo)
* The Pals (RealSubZeroExtabyte)
* The Star Squad Gaming (StarSquadMolly)
* TheEvilShark (StarCode_Animal)
* TheGreatACE (TheGreatAced)
* TheKacperosEN (TheKacperos)
* Thinknoodles (ImNotThinknoodles)
* Thiq Betty (ChrisPurKittiess)
* ThnxCya (NotThnxCya)
* Throbpenz (tharbakin)
* Thumbs Up Family (ThumbsUpFamily)
* ThunberGames (ThunberGames)
* ThyEdgar (THYEDGAR_YT)
* Tigre TV (StarCode_TigreTVyt)
* Tikida (Sanaaa8ans)
* TinenQa (TinenQa1)
* Toxic Berry (NotToxicBerry)
* ToxicJim (supersonshadow17)
* TrafTheOpest (TrafTheOpest)
* Trap Roblox (SUB_toTRAP)
* TroyanoNanoReturns (TroyanoNanoReturns)
* TrustlyDragon (TheTrustlyDragon)
* tsetYT (tsetfed)
* Turtles Wear Raincoats (TurtlesWearRaincoats)
* TussyGames (TussyGames)
* TW Dessi Gaming (TW_Dessi)
* TwiistedPandora (TwistedPandora)
* Tyler & Snowi (snowi_fox)
* Unlimited (Unlimited_Resources)
* Uzoth (Uzoth)
* V2 Plays (MinniiHxpe and toreject)
* Valen Latina (Valen_Latina)
* vanilbean (VanilBean)
* VANILLA (UseStarCode_VANILLA)
* VarietyJay (VarietyJay_Real)
* VeD\_DeV (VeD\_DeV)
* Veyar (VeyarYT)
* ViewSIM (ViewYT)
* VikingPrincessJazmin (VikingPrincessJazmin)
* Viktor (Hihi2234xd2)
* Vindooly (vindooly)
* Vitamine (VitamineRoblox)
* Vitória MineBlox (vitoriamineblox11, anamineblox11, and JackieMineBlox11)
* Volt (v0_1t)
* WaffleTrades (WaffleTrades)
* Waike (JJ_Waike)
* WeirdBlox (WeirdBlox)
* WhiteArrow (YTWhiteArrow)
* WhoseTrade (WhoseTrade)
* WikiaColors (WikiaColor_s)
* WILCO (EsElWilco)
* Willyandgaming (WillyandGaming1)
* Wittyyb (Wittyyb)
* Woozlo (Woozlo)
* XdarzethX - Roblox & More! (xdarzethx)
* Xegothasgot (Xegot)
* xEnesR (xEnesR)
* XenoTy (xXenoTy)
* Xeric (XericPlayz)
* xiaoleung (xiaoleung)
* xMarcelo (xMarcelo)
* Xou (xouzin7)
* Xpie (Xpie)
* Yammy (yammyxox)
* Yayixd Changuito 🐵 (YAYIXDCHANGU)
* Yode Plays (yode1)
* YoSoyLoki (YoSoyLokiYT)
* yTowak (yTowak)
* ZacharyZaxor (ZacharyZaxor)
* ZaiLetsPlay (YT_ZaiLetsPlay)
* Zaryee (Zaryee)
* Zaze Blox (zazebloxx)
* ZeDarkAlien (ZeDarkAlien)
* Zerophyx (Zerophyx)
* Zilgon (Zilgon)
* ZoeTheNoob (z1oee)
* ZOMG (ZOMG)
* ZON (zOnwley)
* ZULY (ItsZulyYT)
* Zurielini (Zurix_500)
* •Lavenderblossom• (OMG4LAV)

### Videos Found
* Axiore (Axiore)
  * Information Collection
    * \[A NEW CODE!\] FREE ROBUX + EVERY WORKING CODES IN BLOX NO ROBLOX:REMASTERED \[SPONSORED VIDEO\]
      * Description references the data collection website robloxwin.com.
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=2oXe2807bDM
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
      * Description references the data collection website earnrobux.gg/online/zone/com.
      * URL: https://www.youtube.com/watch?v=7MyeJoYmXes
    * COMO FICAR RICO NO ROBLOX
      * Description references the data collection website earnrobux.gg/online/zone/com.
      * URL: https://www.youtube.com/watch?v=TPOUmRrXrO0
* DefildPlays (DefildPlays)
  * Information Collection
    * NEW ROBLOX BEACH SIMULATOR RAINBOW CRAB! ! + SHADOW SCYTHE GIVEAWAY WINNER!
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=8c8B8qNVHHg
    * ROBLOX TOYS SERIES 3 BLIND BOXES OPENING AND FREE TOYS CODES!
      * Description references the data collection website rbxfree.com.
      * URL: https://www.youtube.com/watch?v=7CxkZXK3qDE
    * \*NEW\* EASTER EVENT, FREE LEGENDARY EGG, NEW EASTER ORES AND MORE In Roblox Mining Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XWf-oebYlZA
    * (Omg) HOW TO GET FREE THOUSANDS OF HONEY AND FREE ROYAL JELLY IN Roblox Bee Swarm Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ufNwXNRTUUc
    * NEW SANDSTONE REALM, DARK HOLE TOOL AND EMERALD TREASURE In Roblox Treasure Hunt Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=8GzslpLjUoo
    * (Code) NEW ITEM SHOP UPDATE IN ROBLOX FORTNITE! - Island Royale \*Come Join!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=9vEcKNip34s
    * THE 1 CHEST CHALLENGE! - Roblox Fortnite - Island Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=6KJF1VJkW1M
    * (Codes) NEW SPACE REALM AND VIDEO EXCLUSIVE SKIN CODE In Roblox Mining Simulator! \*Insane Money!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=l3OnB5A2DeM
    * (Code) OWNER GAVE ME ALL WORKING SECRET CODES IN ROBLOX MINING SIMULATOR! \*So MANY ITEMS!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Q1iSwPwsjG8
    * NO SCOPE ONLY CHALLENGE In Roblox Fortnite \*Insane!\*- Roblox Island Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XLZ9o5AXVG8
    * 300M LONGSHOTS VICTORY ROYALE - Roblox Fortnite - Island Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ytslSlDFp6s
    * WINNING MY 2 FIRST ROBLOX FORTNITE GAMES EVER! \*Alpha Access Giveaway\* - Roblox Island Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=zzExupK1mPc
    * \*Omg\* BIG INSANE LEGENDARY CHEST OPENING IN ROBLOX BOOGA BOOGA!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=gMHyFZK45kI
    * HOW TO RIDE MAMMOTHS, SHARKS AND EVERY ANIMAL IN Roblox Booga Booga! \*SUPER Insane\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Qx4mQgA5Yas
    * GETTING READY FOR ROBLOX FORTNITE BATTLE ROYALE! \*Come Play\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cIN20sVqkGY
    * BEST WAY TO FIND GOLD AND THIS ITEM COSTS 140.000 ROBUX In Roblox Booga Booga!?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=_W9FDXnLqhk
    * HOW OP IS THE LEGENDARY NUKE IN ROBLOX MINING SIMULATOR! \*4000 Coins Every Second!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=iA6W5QgwNH0
    * (Code) ALL 2018 CODES AND FREE INSANE BACKPACK IN Roblox Mining Simulator! \*1000's Of $\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=nRNzZNeZ_J8
    * (New) ALL 6 UP TO DATE MONEY CODES IN TREASURE HUNT SIMULATOR! \*Easy Money\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=SM4tR3wXhBQ
    * NUKE DELETES ALL SAND ON PRIVATE ISLANDS IN ROBLOX TREASURE HUNT SIMULATOR! \*Insane Sand!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=WHPn7811Oos
    * ROBLOX FORTNITE BATTLE ROYALE RELEASE INFO! \*All You Need To Know\* - Roblox Fortnite
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=nrKIkF6sGWg
    * \*OMG\* THE ULTIMATE ROCKET FUEL CAR RACE IN ROBLOX JAILBREAK!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=XPoWiDKdsEI
    * (How To) AUTOMATICALLY SHOVEL SAND AND GOING TO THE BOTTOM OF ROBLOX TREASURE HUNT SIMULATOR!?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qxUOr1zyUl8
    * HOW TO GET OMNITRIX MASTER CONTROL IN BEN 10 ARRIVAL OF ALIENS! \*Instant Alien Transformation!\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cJX8QjfIhe4
    * HOW TO GET FREE ROCKET FUEL EVERY DAY IN ROBLOX JAILBREAK- New Jailbreak Update
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=bl9J8MJcqUo
    * HOW OP IS THE LEGENDARY GOLDEN SPOON!? \*4000$ EACH SECOND!!\* - Roblox treasure hunt simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=34FOf7O0EAU
    * \*INSANE\* NEW BEN 10 ULTIMATE ALIEN ABILITIES AND ALIEN UPDATES! (Ben 10 Arrival Of Aliens) / Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=9TjcHKkTqPI
    * (Code) NEW BOSS ADDED / ICE CAVE REVAMP / NEW BOSS PETS In Roblox Snow Shoveling Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=H9AO6pa_Y08
    * \[CODES\] \*NEW\* GETTING INSANE LEGENDARY CHESTS IN ROBLOX TREASURE HUNT SIMULATOR?!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=cct9itil1SI
    * NEW SNOWBALL FIGHTS UPDATE IN Roblox Snow Shoveling Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=TXaHCrwFyZM
    * (NEW CODES) ALL \*WORKING\* 2018 CODES in CASH GRAB SIMULATOR - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ICoNfOMsLeY
    * GETTING SHINY CELEBI AND HOOPA AND GENESECT! - POKEMON BRICK BRONZE - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=OfbkOem0hTY
    * (2 Codes) NEW SNOWBALL FIGHTS AND ANIMATIONS UPDATE! Roblox Snow Shoveling Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=C6IL5Jn8C6A
    * HOW TO GET CELEBI AND THE GS BALL IN POKEMON BRICK BRONZE! \*8th Gym Update\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Z-S1uK0Y80s
    * (NEW CODES) ALL \*WORKING\* 2018 CODES in SNOW SHOVELING SIMULATOR - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qmBSq6tz9QU
    * (Code) HOW TO GET A FREE LIGHTSABER AND ICE TOOLS In Snow Shoveling Simulator - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=lq5Kl95HPLY
    * (NEW) WHAT IS INSIDE THE ICE GOLDMINE CAVE IN SNOW SHOVELING SIMULATOR UPDATE! - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=6Ktcb9U1gQU
    * THE 8TH GYM TYPE AND MAGEARNA UPDATE LEAKED!? - POKEMON BRICK BRONZE - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=aiPGBMCQ7Q8
    * \*Epic\* GLITCHED RANDOM ALIEN OMNITRIX BATTLE! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=5ys3SLYPJrs
    * \*INSANE\* GETTING THE MOST OP VEHICLE IN SNOW SHOVELING SIMULATOR! - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=gI9MgqldJpQ
    * ENDLESS HORDES OF ZOMBIES INFILTRATE ROBLOX! \*HELP US\* (Roblox Zombie Attack)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=BKdle4-fB1o
    * I HAVE BECOME ROBLOX IN ROBLOX!? \*SUPER CRAZY\* (Would You Rather..?)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=AnR6tXEljOk
    * Ben 10 DARK OMNITRIX vs LIGHT OMNITRIX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=N99zQHTXaj8
    * \*NEW\* Ben 10 ICE OMNITRIX vs EVIL OMNITRIX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=EPYSupaRxAA
    * \*OMG\* BEN 10 ALIEN ROBS THE NEW TRAIN IN ROBLOX JAILBREAK!? (Ben 10 Jailbreak)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=xVI0kTAnmCg
    * \*NEW\* Ben 10 GHOST OMNITRIX vs NORMAL OMNITRIX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=yPwufoAKBVM
    * \*NEW\* Ben 10 FIRE OMNITRIX vs WATER OMNITRIX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=8cXuXj5RWCw
    * \*Epic\* New LAVA OMNITRIX Random Aliens GLITCH!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=lN1Cl4LOWrE
    * REACTING to "ROBLOX MUSIC VIDEOS: THE MOVIE 2" by Buur
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=-O5QBOz26Fk
    * \*VLOG\* CATCHING NEW GENERATION 3 MONS in Pokémon Go!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=IRPJf9vhmwk
    * \*EPIC\* NEW GOLD OMNITRIX GIVES THESE RANDOM ALIENS!? (Ben 10 Arrival Of Aliens) - Roblox Charity \#8
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=m_q4NkOdLQ8
    * \*OMG\* NEW Alien CHROMASTONE And Ben 10 GAME? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=NPHXyhgDbDk
    * NEW \*SUPER CAR\* And SNIPER In JAILBREAK WINTER UPDATE!! - Roblox Jailbreak
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ON9GgVSfgCs
    * \*EPIC\* NEW RAINBOW Random Alien OMNITRIX! (Ben 10 Arrival Of Aliens) - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=PkolNB3udXY
    * BEN 10 GETS A POKEMON! - Roblox Ben 10 Arrival Of Aliens Roleplay
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=_5Y8-zM2Q_E
    * \*Insane!\* Big Chill EVOLUTION Of Aliens! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=sLlDz299TgE
    * ARCEUS MADNESS! Randomizer NUZLOCKE Challenge! (Pokemon Brick Bronze) \#4
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=k4hjpwtU05I
    * WE GOT ARCEUS! Randomizer NUZLOCKE Challenge! (Pokemon Brick Bronze) \#3
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=VaA2s-TR8Q4
    * MORE UNIQUE POKEMON!? Randomizer NUZLOCKE Challenge! (Pokemon Brick Bronze) \#2
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=pTYUMaWMdhY
    * \*Epic\* How to get ANY LEGENDARY as your STARTER Pokemon In Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=md5GEd-u5_I
    * \*Epic\* NEW Random Alien OMNITRIX!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=AXWGbROJk6o
    * \*Insane\* FASTEST Alien In BEN 10? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=c9S9n5iEf8E
    * \*Insane\* New FREE RARE DAGGER In Swordburst 2! (Ruby Edge Dagger)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=r5GD37uzCG4
    * \*INSANE\* Sword Art Online IN ROBLOX!? (SwordBurst 2)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=o2BqKUXfDxY
    * \*INSANE\* WAY BIG EVOLUTION Of Aliens BATTLE! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ZE8qkXcg3wA
    * \*OMG\* PLAY THE NEW BEN 10 REVAMP!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Cw9sWNIQ4U0
    * NEW ALIENS!! Battle Vs BEN 10 (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=PDiah_BpUvc
    * \*NEW\* ALIENS and future updates! \[Atomix, Rath, Chromastone\] (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=yVB0zoHXOUo
    * \*Secret\* How to get LUGIA in Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=A51X7qCS1Gs
    * GIANT TITAN TROLLING in TITAN SIMULATOR! (Roblox Titan Simulator)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ufS-xFYuR2k
    * BACON HAIR FINDS HIDDEN LEGENDARY GATE IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jvCb7-RJPB0
    * NEW EVOLUTION OF ALIENS MODE! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ZrKYuLfxwpI
    * EPIC NEW ALIENS! BATTLE VS BEN 10 (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ALjuldoBk3s
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
    * BACON HAIR FINDS LEGENDARY PORTAL IN POKEMON BRICK BRONZE \*Halloween Event\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Fy0X4dIWH4Q
    * GETTING SHINY HALLOWEEN EVENT POKEMON IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=WcDy77qWfqM
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
    * GET YOUR OWN AIRPLANE IN POKEMON BRICK BRONZE!? \*Omg\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=NRbPWR-3F_w
    * BACON HAIR GIVES THESE FREE LEGENDARY EGGS IN POKEMON BRICK BRONZE! \*Trolling\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=z1klRQ8noDA
    * THE LAST ROBLOX GUEST HAS THIS IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=z578Db6HWjs
    * BACON HAIR HAS THESE SHINY LEGENDARIES IN POKEMON BRICK BRONZE?!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=7HNUNxqgL2g
    * THIS ITEM COSTS OVER 1 MILLION DOLLARS IN JAILBREAK!? - MASSIVE UPDATE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ePW0W6Zufkk
    * LEGENDARY KNIFE CASES OPENING IN ROBLOX PHANTOM FORCES
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=bPxhgik49Hw
    * POKEMON PARTY IS BACK?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=wee811lRl5s
    * GWEN 10 MAGIC SPELLBOOK IN ROBLOX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=JIF09YwNSaU
    * BACON HAIR BATTLES THIS INSANE TRAINER IN POKEMON BRICK BRONZE!?
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=iZ7axNgI4eM
    * BACON HAIR FINDS PORTAL TO THESE LEGENDARIES IN POKEMON BRICK BRONZE!!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Gr5SJMj27f4
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
    * HOW TO MAKE 500.000$ EVERY HOUR IN POKEMON BRICK BRONZE!!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=UP-Q00zCrvw
  * Non-Giftcard Robux Giveaways
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
    * I DEFEATED THANOS AND GOT INFINITY SUIT IN SUPERHERO SIMULATOR UPDATE! Roblox \*code\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=KjWnZlBSlVM
    * I GOT SECRET PETS AND MYTHICAL CONE HAT IN UNBOXING SIMULATOR! Roblox \*INSANE LEVELS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=WccVpULAEmo
    * QUEST GEMS AND EGG GAMEPASS UPDATE CODES IN UNBOXING SIMULATOR! ROBLOX
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ANYwugz6m80
    * SECRET CONSTRUCTION UPDATE COIN CODES IN UNBOXING SIMULATOR! Roblox \*21 OC COINS!\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zxttJfqRqxU
    * ALL 20 SECRET UPDATE BOOST CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=YVWY4HqC5C0
    * ALL SECRET SPEED CODES IN LEGENDS OF SPEED SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=F6cFwF-uXqs
    * RAINFOREST MISC CHEST AND OP GODLY HAT IN UNBOXING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Qo0Frzw5JXE
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
    * x100 ENCHANTING, F2P IS NOW OP IN UNBOXING SIMULATOR UPDATE!! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=X5Pl7Hb8OqY
    * ALL CODES AND HIDDEN LAVA BLADE LOCATION IN TREASURE QUEST SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=H151aUfHipM
    * ALL 24 OWNER CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=zxACdW7AMww
    * 3 CODES, MAKING ALL BOXES PRESENTS AND THIS HAPPENED.. IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=8hEN0q1bTFU
    * BEST 4X EXP LEVELING GUIDE AND 2 CODES IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=K2AqeYMhSY8
    * SUMMER SECRET PET EGG UPDATE CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=jGfrt6pJav8
    * SECRET TOY BOOST CODES IN UNBOXING SIMULATOR! Roblox \*30 SX COINS!?\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=hyIsIxg9tXc
    * ALL 20 YOUTUBER CODES AND CRAFTING UPDATE SOON IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=1RwJRR-JCXI
    * ALL 36 SECRET OWNER CODES IN UNBOXING SIMULATOR! Roblox \*INSANE BOOSTS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=W1q8TVRDjiM
    * SECRET ICE KINGDOM CODES AND GIANT FROST EGG IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=VR6ZpkGCjZw
    * 25 PRO PLAYERS VS DUNGEON IN UNBOXING SIMULATOR! \*330 TRILLION DMG\* Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Kc5VTL4x-oQ
    * SECRET PET CODE AND MAGMORAUG BOSS IN GHOST SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=ma1rnqA_V5M
    * LEVEL 6000+ MYTHICAL HATS AND PETS IN UNBOXING SIMULATOR! Roblox \*INSANE\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=2BEQfqLJwtY
    * ALL 20 SUPER CODES IN UNBOXING SIMULATOR! Roblox \*SUPER OP\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=VsVTjt2fLEE
    * OP OWNER 35,000 TRILLION COINS CODE IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=4qSTZKJ-RGY
    * SECRET OWNER PET CODE AND OP GODLY PETS IN GHOST SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=i1Jc2u3zXuw
    * SECRET BEST WAY TO LEVEL PETS FAST IN UNBOXING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=sZ8A77W48No
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
    * NEW EASTER BASKET SECRET PET IN ROBLOX BUBBLE GUM SIMULATOR UPDATE!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=l1Jiw1yyPU4
    * ALL SECRET DRILLING SIMULATOR CODES! Roblox \*FREE PETS!\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=U4nbCuVZNUc
    * I GOT BEST ATLANTIS GUARDIAN PET IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Ux5-77M_RS0
    * ALL BUILDING SIMULATOR SECRET CODES! Roblox \*GIANT TOOLS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=skGrahFetk4
    * ALL BOKU NO ROBLOX REMASTERED CODES! \*FREE\* SECRET QUIRKS!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=CBjQRdr3P6o
    * PET SIMULATOR + POKEMON = PET TRAINER SIMULATOR!? Roblox \*INSANE\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=QuE7pYEZ5kU
    * I GOT THE BEST GODLY PETS IN PET DIGGING SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=uLE1JKm4YNA
    * ROBLOX PIZZA PARTY EVENT! FREE EXCLUSIVE CREATOR PURPLE PARTY AFRO!!
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=b_Dz89TbXzw
    * SECRET CODES MADE ME THE BIGGEST BABY ON BABY SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=WbSd2oblbII
    * THESE SECRET CODES MADE ME OP IN RPG WORLD SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=HpBYlT1QO4A
    * SHINY POT O' GOLD, LUCKY OVERLORD PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=sp8ojY529WY
    * LEGENDARY LUCKY DOMINUS CODES IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Pbe-9D2x47U
    * I GOT THE BEST SPELLS IN ROBLOX DUNGEON QUEST! \#1
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=R5bN4fX_2jk
    * I OPENED 10.000 WATER EGGS GOT THESE PETS IN BUBBLE GUM SIMULATOR!? Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=E-CA98I1FAY
    * GET FREE SEA URCHIN PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=IdkkW_7inhc
    * RAREST SHINY CLASSIC TRIO PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dpqLLPLQx9U
    * LEGENDARY SEA URCHIN BEACH CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=fy5b8YNVNVg
    * LEGENDARY LIGHT SERPENT PET CODES IN PET RANCH SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=B9ZAb_dyAFA
    * 5 ENCHANT BOOST CODES IN BUBBLE GUM SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=38uzCzRC6xM
    * GODMODE AND PRESTIGE PETS IN SLAYING SIMULATOR UPDATE! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=dPolwOt4JWQ
    * GETTING THE BEST RANDOM SHINY PETS IN BLOB SIMULATOR 2! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=evZERi3yenI
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
    * R.I.P HELICOPTER + CODES IN ROBLOX MAD CITY! \*GAMEPASS GIVEAWAYS\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Qq-MsU0Mqfc
    * FREE ELECTRIC HYDRA DOMINUS IN BUBBLE GUM SIMULATOR!? Roblox \*GamePass Giveaways\*
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=AijCwOhOH4I
* Denis (DenisDaily)
  * Information Collection
    * SIR MEOWS A LOT TAKES OVER ROBLOX!
      * Description references the data collection website growbux.net.
      * URL: https://www.youtube.com/watch?v=dwbGKuIDfxA
* DimerDillon (TheDimer)
  * Other
    * Teleporting Simulator! (FUCK FUCK FUCK)
      * Video, title, and description contain the "F word" 8 times.
      * URL: https://www.youtube.com/watch?v=rh_sr6rEWWI
* Erik Carr (ErikCarrKZ)
  * Information Collection
    * ATUALIZAÇÃO! O Mapa Me Jogou pra 9.999.999 METROS DE ALTURA - Roblox Obby Troll
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=LS8ElNi07ZQ
    * Comprei um HIDROAVIÃO no AEROPORTO - Roblox Airport Tycoon
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=STX7KQ1K67I
    * EM BUSCA DA COROA no CORREDOR DOS YOUTUBERS - Roblox Corridor of YouTubers
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=IfLg5Ic8ycc
    * Comprei um NAVIO de 5 MILHÕES - Roblox Airport Tycoon
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=LLaiKDSPa6s
    * VIAJANDO em PORTAIS DO UNIVERSO - Roblox Portal Rush
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=ty879ZN-i7Q
    * NOVO MAPA do FALL GUYS no ROBLOX - Slip Blox
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=GUTeFsZ4CGA
    * Subindo a ESCADA IMPOSSÍVEL - Roblox Climb Time
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=iQzMjbx57o0
    * FÁBRICA DE LUCKY BLOCK - Roblox Lucky Block Tycoon
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=54YF6VFY0Mg
    * Maneira mais FÁCIL de conseguir ROBUX GRÁTIS + SORTEIO
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=h8QnGhwODVM
    * CADA NÍVEL, MAIS DIFÍCIL - Roblox Obby 9999 NÍVEIS
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=fE42wynwnO0
    * PIGGY CAPÍTULO CASA GIGANTE! (Piggy Mapa dos Inscritos)
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=eL87_8lKMaY
    * PIGGY CAPÍTULO MAIS DIFÍCIL! (Piggy Mapa dos Inscritos)
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=m7tZGgDhVrs
    * NOVO CAPÍTULO PIGGY? (Zoológico) - Roblox
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=XYLMDSgAqS4
    * Piggy Simulator CAPÍTULO 3 + VIP - Roblox
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=Lbj2a2NsfP0
* Geko97 - Roblox (Flexer97YT)
  * Information Collection
    * Sell roblox via online • Tools for Ecommercee• Affiliate Marketing• Passive Income• Shopify•
      * Description references the data collection website rbxcash.com.
      * URL: https://www.youtube.com/watch?v=A_NIUghaUM0
    * HOW TO CREATE A PASSIVE INCOME GAMING 34 \#passiveincome \#gaming
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=lVRkb90j_go
    * HOW TO CREATE A PASSIVE INCOME GAMING 35 \#passiveincome \#gaming
      * Description references the data collection website bloxwin.com.
      * URL: https://www.youtube.com/watch?v=l3qwVIOIwgc
* HelloItsVG (HelloItsVG)
  * Information Collection
    * \[PROMO CODE\] How To Get Jurassic World Shades - Free Roblox Promo Code for Creator Challenge Event
      * Description references the data collection website oprewards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=shxjti72b7g
    * BEST WAY TO TROLL IN APRIL FOOLS!
      * Description references the data collection website oprewards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=3AQYy6XdNpI
* JD (Thexz)
  * Phishing
    * HACKING A ROBLOX ACCOUNT AND ADDING ROBUX!!!
      * URL: https://www.youtube.com/watch?v=_XmbkXyoBeA
* Lyna (Lynitaa)
  * Non-Giftcard Robux Giveaways
    * REGALO 100.000 ROBUX SI PIERDO ESTE RETO EN ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=ZNZLqUPWnfc
* MIKEYDOOD (IMMIKEYDOOD)
  * Phishing
    * HACKING MY VIEWER AND SPENDING ALL OF HIS ROBUX!!!
      * URL: https://www.youtube.com/watch?v=j3ao8hO0ANI
  * Non-Giftcard Robux Giveaways
    * GIVING AWAY 15,000+ ROBUX FOR... (Roblox)
      * Video announces a Robux giveaway.
      * URL: https://www.youtube.com/watch?v=df0NiP-SRUQ
    * EASIEST WAY TO EARN ROBUX FROM ME!!! \*FREE ROBUX GIVEAWAYS\*
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=WZ7dbUBOePA
* RAMBLING RAMUL (RamblingRamul)
  * Non-Giftcard Robux Giveaways
    * \[ROBLOX\] WIN FREE ROBUX IN MY LOTTERY IN SURVIVOR
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=-oDhbZUAa6Q
    * \[ROBLOX\] PLAYING JAILBREAK & SURVIVOR + WIN FREE ROBUX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=GH6V19WScUM
    * \[ROBLOX\] WIN FREE ROBUX Every 10 Min In My Lottery In Jailbreak
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=IvEQ8DO7pj0
    * \[ROBLOX\] WIN FREE ROBUX IN MY LOTTERY EVERY 10 MIN IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=AsezfTUi0yM
    * ROBLOX JAILBREAK + WIN FREE ROBUX EVERY 10 MIN IN MY LOTTERY
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=tzWEBdn0ECk
    * \[ROBLOX\] WIN FREE ROBUX IN MY LOTTERY IN SHARKBITE & SURVIVOR
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=4da7gz3CWCI
    * ROBLOX JAILBREAK + ROBUX LOTTERY EVERY 10 MIN
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=AsugmojDkd8
    * WIN FREE ROBUX EVERY 10 MIN IN MY ROBLOX LOTTERY IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=G0Z3AsOxXVk
    * WIN FREE ROBUX IN MY ROBUX LOTTERY IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=4BzaebIDLbA
    * PLAYING JAILBREAK & SURVIVOR IN ROBLOX + FREE ROBUX LOTTERY
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=IhEZtNsGkqQ
    * WIN 50 FREE ROBUX EVERY 10 MIN IN MY LOTTERY IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=RW2zbz1AaBs
    * WIN FREE ROBUX EVERY 10 MIN IN MY ROBLOX LOTTERY IN SURVIVOR
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=OyI5cirJMJ8
    * FREE ROBUX LOTTERY EVERY 10 MIN IN JAILBREAK ROBLOX (NOT FAKE)
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=oeyvoqUrgBo
    * WIN ROBUX EVERY 10 MIN IN MY ROBUX LOTTERY IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=vfPMgPISvFk
    * WIN FREE ROBUX EVERY 10 MINUTES IN JAILBREAK ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=BUbVGmQuTFg
    * ROBUX GIVEAWAYS IN JAILBREAK ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=TIt5NsP9meo
    * FREE ROBUX GIVEAWAY EVERY 10 MIN IN JAILBREAK ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=OHWXW2_NbYU
    * FREE ROBUX GIVEAWAY EVERY 10 MIN IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=BfsaB988chk
    * FREE ROBUX GIVEAWAY EVERY 10 MIN IN JAILBREAK ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=I-7JLjx6bMY
    * THIS IS THE ONLY WAY TO GET FREE ROBUX IN ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=WLXXgHH0E5U
    * GIVING AWAY 50 ROBUX EVERY 10 MIN IN JAILBREAK ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=bAgiH3DH0Jo
    * WIN 50 ROBUX EVERY 10 MIN IN ROBLOX JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=6XGaZvn04nQ
    * REAL ROBUX GIVEAWAY IN JAILBREAK ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=IpnMtA9Zxpk
    * ROBUX GIVEAWAY EVERY 10 MIN IN SURVIVOR ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=_jpW8uAc1as
    * 50 ROBUX GIVEAWAY EVERY 10 MIN IN JAILBREAK ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=vlYsD5-kLSs
    * ROBUX GIVEAWAY EVERY 10 MINUTES IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=ap3RpSGDO58
    * JAILBREAK ROBLOX / NEW ESCAPE / GIVING AWAY ROBUX EVERY 10 MIN
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=M33jodDtnXg
    * NEW ESCAPE JAILBREAK UPDATE IN ROBLOX & ROBUX GIVEAWAY EVERY 10 MIN
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=SUfZeRl3HJA
    * GIVING AWAY 50 ROBUX EVERY 10 MIN IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=CDKQW-eonos
    * ROBUX Giveaway EVERY 10 Minutes While Robbing Stores In JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=XhOwzJJRapk
    * GIVING AWAY 50 ROBUX EVERY 10 MIN IN JAILBREAK ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=4bqVPzUXueU
    * GIVING AWAY 50 ROBUX EVERY 10 MIN IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=1YcewCkwSvY
    * JAILBREAK ROBBING GAS STATIONS UPDATE + ROBUX GIVEAWAY EVERY 10 MIN
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=H3Rtpt7Ew10
    * ROBLOX JAILBREAK / GIVING AWAY 50 ROBUX EVERY 10 MINUTES
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=yhrqIuutfoo
    * GIVING AWAY FREE ROBUX EVERY 10 MINUTES IN SURVIVOR
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=qU9jTvjY4AU
    * GIVING AWAY 50 FREE ROBUX EVERY 15 MINUTES IN POKEMON BRICK BRONZE
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=s6cddtzp2Oo
    * GIVING AWAY FREE 50 ROBUX EVERY 15 MINUTES IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=FbOEXeYfQE8
    * GIVING AWAY FREE ROBUX EVERY 15 MINUTES IN POKEMON BRICK BRONZE
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=yiHyYxDJHMs
    * Giving away free Robux every 15 minutes in Survivor
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=7EwgFZC2p2c
    * GIVING AWAY FREE ROBUX EVERY 15 MINUTES IN JAILBREAK
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=lr0jA_C20oE
    * Get Free Robux
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=cDc6MjxNOU0
    * ROBUX GIVEAWAY + PLAYING SURVIVOR IN ROBLOX / COME PLAY
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=K8WSic-SaeI
    * PLAYING SURVIVOR IN ROBLOX ON PRIVATE SERVER + ROBUX GIVEAWAY
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=dG6EKXAI1kE
    * POKEMON BRICK BRONZE / CHANSEY + PORYGON + ROBUX GIVEAWAY
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=JoJHPnSHP1M
    * SURVIVOR IN ROBLOX + MORE ROBUX GIVEAWAYS
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=h03Aa-0zw74
    * PLAYING SURVIVOR IN ROBLOX + ROBUX GIVEAWAY
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=YCScIXuPVl4
    * HUNTING FOR LEGENDS IN POKEMON BRICK BRONZE + ROBUX GIVEAWAY
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=D9X_d1UzEKI
    * GIVING AWAY 50 ROBUX EVERY FEW MINUTES / PLAYING PBB & ZOMBIE RUSH / ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=DZUOXsVkcRY
    * GIVING AWAY ROBUX & PLAYING SOME POKEMON BRICK BRONZE | ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=GjgJXJqI1T4
    * GIVING AWAY 50 ROBUX EVERY FEW MINUTES or so.. / PLAYING VARIOUS GAMES / ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=6XJ7Oj2TE4M
    * POKEMON BRICK BRONZE / HUNTING FOR LATIOS / FREE ROBUX GIVEAWAY
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=ggcZYwlXvhU
    * MANAPHY EGG HUNT + 2400 ROBUX GIVEAWAY / POKEMON BRICK BRONZE
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=q1nFZb81xCo
* SeeDeng (SeeDank)
  * Phishing
    * PLAYING ON A FAN'S ACCOUNT IN ROBLOX (SPENDING ALL THEIR ROBUX)
      * Logs into the account of a fan. Mentions a lot of other people sent their username and passwords.
      * URL: https://www.youtube.com/watch?v=9qOAqB6E-z8
* Smurfzin (Smurfzineo_YT)
  * Information Collection
    * COMO CONSEGUIR SEGUIDORES INFINITOS NO ROBLOX!! ✔️
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=1VXEKlaIdgM
    * TROLLANDO COM HACK NO AMONG US
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=FzNVCDLvyGg
    * FAZENDO O AVATAR DO LIL PUMP NO ROBLOX!!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=FoNBJep7oSE
* Stron (StronbolDev)
  * Information Collection
    * PROBANDO PAPAS ¿RARAS? DE OTROS PAISES 😱 \*CON MI CARA\*
      * Video is about the informtion collection website ezrewards.today.
      * URL: https://www.youtube.com/watch?v=KI5O1tRgekU
    * YOUTUBERS VS ILUMINATI VIDEO REACCION EPICA CON XONNEK \*MUESTRO MI CARA\*
      * Contains an ad for the information collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=gYBSdkTFOtY
* Suliin18YT (Suliin18YTT)
  * Non-Giftcard Robux Giveaways
    * MI PRIMER MASCOTA Y COMPRO LA CASA DE HADAS - ADOPT ME ROBLOX
      * Mentions Robux giveaways through nimo.tv.
      * URL: https://www.youtube.com/watch?v=M5lLteKfC8Y
    * 😎 COMPRO LA NUEVA MANSIÓN DE CELEBRIDADES EN ADOPT ME - ROBLOX
      * Mentions Robux giveaways through nimo.tv.
      * URL: https://www.youtube.com/watch?v=_xHeiSnXJEo
* TanqR (TanqR)
  * Non-Giftcard Robux Giveaways
    * 1 KILL = $1000 Robux in Roblox Bedwars..
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=COqizjAstxE
* ZephPlayz (Zeph)
  * Information Collection
    * Easy Way To Get Robux Without Money! (Roblox)
      * Description references the data collection website oprewards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=dBUnHb_1kFo
    * MEETING JAILBREAK'S OWNER! OMG! (Roblox)
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=N-NV4DX7aTI
    * DanTDM Said THIS About Roblox
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=eb5l7WaM3Jo
    * Why DanTDM Quit Roblox
      * Description references the data collection website flame.gg.
      * URL: https://www.youtube.com/watch?v=H3id5wKBPX4
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
      * Description references the data collection website oprewards.com.
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
    * MAKING JAMES CHARLES a ROBLOX ACCOUNT
      * Includes the words "Sex Tape" at 2:53
      * URL: https://www.youtube.com/watch?v=3P6LVJBFkGg
    * I Said \*\*\*\* For The First Time.. (Roblox)
      * Video includes  "f\*\*\*" (2x), "b\*\*\*\*", and "bulls\*\*\*" included but bleeped out. Middle fingers are also used at the end.
      * URL: https://www.youtube.com/watch?v=t_W8ReEfuHY
  * Phishing
    * I HACKED A FANS ACCOUNT AND GAVE THEM ROBUX! | Roblox
      * URL: https://www.youtube.com/watch?v=z_a7yQ9oVuk

## Conclusion
The inconsistent enforcement of the Roblox terms of use and laws in the United States needs to be addressed to improve
community safety and improve the relations of the influencer program and developers. Robux giveaways and phishing can
give legitimacy to malicious people trying to get access to users' accounts while information collection puts the
information of minors at risk. Addressing these will make developers have more confidence in moderation and Roblox's
marketing team.