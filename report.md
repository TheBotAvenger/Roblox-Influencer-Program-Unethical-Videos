# Roblox Influencer Program Unethical Videos
Generated September 30, 2025<br>
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
* Total videos: 1,048,948 videos
* Total videos found that match keywords: 58,332 videos
  * Total unprocessed videos: 12,308 videos
* Total videos found that are processed and marked: 243 videos 
  * Information Collection: 198 videos
  * Non-Giftcard Robux Giveaways: 42 videos
  * Phishing: 2 videos
  * Other: 1 video

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
* BeaPlays (notBeaPlays)
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
* Biel Henrique (BielHenriikOficial)
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
* Danny Jesden (DannyJesdenYT)
* DanteField (etnad34)
* DaPandaGirl (Da_PandaGirl)
* DatBrian (DatBrian)
* Daylins Funhouse (DaylinsFunhouse, FunHouseDadWasTaken, and Fun_HouseMom)
* DeeterPlays (DeeterPlays)
* DefildPlays (DefildPlays)
* DeGoBooM (BumiReal)
* Denis (DenisDaily)
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
* DUDU Betero (DUDUBetero\_ofcYT, DuduBetero, RafinhaBetero\_BTR, and RafaBetero)
* DV Plays (DVwastaken)
* El Magnum (MagnumStormus)
* elDanielGT (DanielGTReal)
* ElemperadormaxiYT (EmperadorMaxiOFICIAL)
* Elite (E1ite_E)
* Elitelupus (elitelupus)
* Ella (ellasparis)
* EmpireBlox (EmpireBloxOficial)
* Erik Carr (ErikCarrKZ)
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
* Geko97 - Roblox (Flexer97YT)
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
* HelloItsVG (HelloItsVG)
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
* It's Akeila (itsakeila)
* It's Siena (ItsNotSiena3)
* iTownGamePlay \*Terror&Diversión\* (iTownGamePlayYT)
* Its Matty (YT_ItsMatty)
* Its Starlight plays YT (Its_starlightplaysYT)
* Its\_CxldKid (Its\_CxldKid)
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
* JD (Thexz)
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
* MIKEYDOOD (IMMIKEYDOOD)
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
* MrPankeyk (MrPankeyk)
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
* RAMBLING RAMUL (RamblingRamul)
* Rax (RaxBLX)
* Razor (mprazor)
* RazorFishGaming (UseCodeRAZORFISH and RazorFishPengi)
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
* Stron (StronbolDev)
* STUD (s7ud)
* Sugar (ourfire and ChrisOurFire)
* SulyMazing (iamtheonepoundfish)
* SunnyxMisty (xSunnyxMistyx)
* Sunset Safari (HeySunsetSafari)
* SuperDog Tyler (superdog_tyler)
* SweePee (SweePeeRBLX)
* Só Por Causa (Est3vA0)
* Sören Abbaok (Abbaok)
* Tangochini (Tangochini)
* TanqR (TanqR)
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
* ZephPlayz (Zeph)
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
* Cerso (Cerso93)
  * Non-Giftcard Robux Giveaways
    * SORTEO DE ROBUX PARA ROBLOX GRATIS | Cerso Roblox en español
      * Uses a Roblox Group with group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=V5Y3MNZv4K8
* DimerDillon (TheDimer)
  * Other
    * Teleporting Simulator! (FUCK FUCK FUCK)
      * Video, title, and description contain the "F word" 8 times.
      * URL: https://www.youtube.com/watch?v=rh_sr6rEWWI
* IntelPlayz (IamRoBuilder)
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
    * HUGE PUMPKIN!! | EPIC BOAT | Roblox Build A Boat For Treasure
      * Description references the data collection website bloxmarket.com.
      * URL: https://www.youtube.com/watch?v=SmgFuI7UzbM
    * Buying Super Powers For 5 Fan! (50,000,000$) Lumber Tycoon 2
      * Description references the data collection website bloxmarket.com.
      * URL: https://www.youtube.com/watch?v=tbKapJPBUNY
* JustHarrison (JustHarrisonYT)
  * Non-Giftcard Robux Giveaways
    * 🔴 DOING THE 20,000 ROBUX GIVEAWAY LIVE!!!
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=F6SpRGVTviE
    * (ENDED) 5 PEOPLE WILL WIN 4,000 ROBUX EACH!!! (Tower Defense Simulator - ROBLOX)
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=U8HJSDkPabY
    * (OVER) 2,500 ROBUX GIVEAWAY!!! LAST CHANCE TO GET TOXIC GUNNER (Tower Defense Simulator - ROBLOX)
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=Qnq6AuArpdM
* Lyna (Lynitaa)
  * Non-Giftcard Robux Giveaways
    * REGALO 100.000 ROBUX SI PIERDO ESTE RETO EN ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=ZNZLqUPWnfc
* Poke (Pokediger1)
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
* Realistic Gaming (Starcode_RealisticG)
  * Information Collection
    * HOW TO GET FREE ROBUX in 2018! APP THAT GIVES YOU ROBUX FOR PLAYING GAMES! FREE ROBUX 2018
      * Description references a download for a data collection mobile app.
      * URL: https://www.youtube.com/watch?v=ESjjCV9WXW8
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
* Suliin18YT (Suliin18YTT)
  * Non-Giftcard Robux Giveaways
    * MI PRIMER MASCOTA Y COMPRO LA CASA DE HADAS - ADOPT ME ROBLOX
      * Mentions Robux giveaways through nimo.tv.
      * URL: https://www.youtube.com/watch?v=M5lLteKfC8Y
    * 😎 COMPRO LA NUEVA MANSIÓN DE CELEBRIDADES EN ADOPT ME - ROBLOX
      * Mentions Robux giveaways through nimo.tv.
      * URL: https://www.youtube.com/watch?v=_xHeiSnXJEo
* WhiteArrow (YTWhiteArrow)
  * Information Collection
    * JAMÁS TENDRÁS NOVIO ​⁠@customuse3D \#customuse \#RobloxUGC \#robloxoutfits \#howtomakeRobux \#shorts
      * Description references the data collection website makerobux.co.
      * URL: https://www.youtube.com/watch?v=7yqyWgY7Mvg

## Conclusion
The inconsistent enforcement of the Roblox terms of use and laws in the United States needs to be addressed to improve
community safety and improve the relations of the influencer program and developers. Robux giveaways and phishing can
give legitimacy to malicious people trying to get access to users' accounts while information collection puts the
information of minors at risk. Addressing these will make developers have more confidence in moderation and Roblox's
marketing team.