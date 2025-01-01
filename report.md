# Roblox Influencer Program Unethical Videos
Generated January 1, 2025<br>
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
* Total videos: 956,861 videos
* Total videos found that match keywords: 56,514 videos
  * Total unprocessed videos: 10,543 videos
* Total videos found that are processed and marked: 153 videos 
  * Information Collection: 118 videos
  * Non-Giftcard Robux Giveaways: 34 videos
  * Phishing: 1 video

### No Videos Found
The following channels had nothing appear with manual searching. Videos may exist, but were not found.
* 39jeshi (39jeshi)
* 3SB Games (cakemiix and 3SBMiichael)
* Aati Plays (AatiPlays_Official)
* Abbaok (Abbaok_YT)
* AbsintoJ (AbsintoJYT)
* Acenix (AcenixElMejorYoutube)
* AEREN (AaronDRonin)
* AidanGamerHD (AidanGamerHDXD)
* AL3XEY (AL3XEEY)
* Alaska Violet (alaskaviolett)
* Alex Crafted (HeyCrafted)
* Aline Games (AliineGamesYT)
* AlphaGG (RealYouTube_AlphaGG)
* AlvinBlox (Alvin_Blox)
* Aly1263 (aly1263)
* Amberry (Amberrry)
* Andre Nicholas (andrhevn21)
* Andshot (Andshotx)
* Angelazz (Angelazz)
* Angeltanked (angelchopped)
* AnielicA (ANIELICA01)
* Anime Revolt (RedWEBE3)
* Ant (Cringley)
* Ant Antixx (Ant_Antixx)
* ApplyingTM (ApplyingTM)
* Argo Play (YT_Argo)
* arielle (ariellesyt)
* Armenti (LeArmenti)
* Ashley (codeeunicorn)
* AshleyBunni (BunniYT)
* AtlasZero (AtlasBlox7)
* Auratix (Auratix)
* Avocado Playz (AvocadoPlayzOfficial)
* Awhxliv (TheRealAwhxliv)
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
* BeeBlox (ThePapaBee, imgabibee, MrBeeWasTaken, and LaMamaBee)
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
* Brigido (oCauanBrigido)
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
* Carol TV (caaroltv)
* CatFer (SoyCatFer)
* CATYPINK PLAY (catypink15)
* cazum8 (cazum8)
* Ceddy (CeddyCrew)
* Cee\_berry (Cee\_berry)
* Cerso (Cerso93)
* Chekecheto (Chekelete1)
* Cheo Power (cheopower)
* CherryAhrizona (baby_ahrizona)
* Chizeled (Chizeled_YT)
* Chocoblox (Chocoblox_93)
* Chrisandthemike (chrisatm)
* Christocream (Christocream)
* Chrisu (ChristsuYT)
* cKev (cKevvv)
* Claus (ClausOFC)
* Cleanse Beam (CleanseBeam)
* ComfySunday (ComfySunday)
* Complex Roblox (IamComplexRoblox)
* Conor3D (Conor3D)
* Cookie Cutter (CookieCutterYT)
* corny (co_rny)
* Cosmic (bluecosmiic)
* Crainer (MrCrainer1422)
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
* DimerDillon (TheDimer)
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
* DUDU Betero (Pao_Paodequeij0, DuduBetero, SimoneBetero, and RafaBetero)
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
* FadedPlayz (YTFadedPlayz)
* faeglow (faeglow)
* FakerUp (NotFakerUp)
* Fanaticalight (Fanaticalight)
* FancySmash (RealFancySmash)
* faulty (Faulltyy)
* febatista (febatistaYT)
* fer999 (StarCode_fer)
* Fera (Fera0ficial)
* FGTeeV (FGTeeV and DrizzMcNizz)
* Fini Juega (FiniJuega)
* FishyBlox (FishyBloxYT)
* FlameRBX (SnowRBX_Youtube)
* Flamingo (mrflimflam)
* flashii (flashiilindo)
* Flebsy (Flebsy)
* FloatyZone (FloatyZone)
* Flxral (xoFlxral)
* FlyBorg (KaykBlox)
* Fminus (fminusmic)
* Folix (Mrpresident1302)
* Foltyn (TheRealFoltyn)
* Foncri (Foncri)
* Fraser2TheMax (Fraser2TheMax)
* frenchrxses (french_rxses)
* FUDZ (fudsim)
* FunkySquadHD (UseCode_Funky)
* FunnyBunny (Jxssivca)
* FunPiggy (CelestialPiggy)
* Furious Jumper (furi0us_jumper)
* FusionZGamer (NotFusionZGamer)
* Gaby Gamer (GabyGamerrOficiall)
* Gallant Gaming (GallantGaming)
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
* HandN (xHandNx)
* hannnahlovescows (hannnahlovescows)
* HappyBlack (HappyThreePro)
* HelloItsVG (HelloItsVinh)
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
* IBella (Cinderbelle)
* iBeMaine (ibemaine)
* iBugou (iBugouzinho)
* iDatchy (iDatchy)
* iKotori (iKotori)
* ImaGamerGirl (REALImaGamerGirl)
* InceptionTime (InceptionTime)
* Inemafoo (Inemajohn)
* InquisitorMaster (inquisitormaster)
* IntelPlayz (IamRoBuilder)
* Irmãs Gamers (iiPietra_GamesYT)
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
* Jello Queen (JelloQueenYT)
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
* Julianbank 2 (blvckbank)
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
* Kitt Gaming (StarCode_kitt)
* Kodak (KodakOF)
* KraoESP (KraoESP)
* Kreative Kyle (Kreative_Kyle)
* KreekCraft (KreekCraft)
* Krystin Plays (KrystinPlays)
* KTG Gaming (lifetimeobcman123)
* KunicornCraft (kawaii_kunicorn)
* Kvssidy (kvssidy)
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
* locus (locus200k)
* LOGinHDi (L0GinHDi)
* Lokis (lokis9340)
* Lonnie (GPR3)
* Lord (Lorrd_ofc)
* LordMetalizer (MetalizerRBX)
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
* MeganPlays (TheMeganPlays)
* MelzinhaMel Games (MelzinhaMelGames, Detona\_Anderson, and Princesa\_Alicinha)
* Mia Games (MiaZaffyt)
* MIANNN (MIANNNGAMER)
* MICHI RØBLØX (michineyley)
* MicroGuardian (MicroGuardian)
* MIKEYDOOD (IMMIKEYDOOD)
* Mila FunPlayer (trxmila)
* MiniBloxia (SubToMiniBloxia)
* MM2 Chillz (ChillzSubZero)
* Model8197 (Modeldog8197)
* MoFlare (MoFlare)
* Moody Unicorn Twin - Roblox (moodyunicorntwin)
* Moonfallx (moonfallx)
* Mr. Bunny (AlanConejito_7u7)
* Mr\_Booshot (Mr\_Booshot)
* MrLokazo86 (Lokazo86)
* MrPankeyk (MrPankeyk)
* Mud (DunkinMud)
* Muneeb (ItsMuneeeb)
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
* NightFoxx (Night_foxx)
* Ninjagaming (NinjagamingRBYT)
* NO\_DATA (NO\_DATA)
* Noob Master (N00B_Mast3rXD)
* NotAmberRoblox (NotAmberRoblox)
* notive (notive)
* O1G (TotallyNotO1G)
* ObliviousHD (ObliviousHD)
* officialnoobie (oofficialnoobie)
* oGVexx (LouDaREALGamer)
* OKEH SQUAD (Rbellomello)
* Olix (OIixYT)
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
* PHMittens (PHMittensSTARCreator)
* Phoeberry (Phoeberry)
* PinkFate Games (PinkFateYT)
* PinkLeaf (RenLeaf)
* Play with me - Apps and Games (da\_dania and kaan\_xy2)
* Plech (PlechitoYT)
* Plique Games (PliqueYT)
* Poke (Pokediger1)
* PolarCub (polarcub_art)
* Premiumsalad (premiumsalad)
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
* Robuilds (Rxbuilds)
* Rod Velloso (viddrox)
* RODNY (RODNY_ROBLOX)
* Rovi23 (byRovi23 and MelLovesBunnys)
* roxhi roblox (12345roxyhi12345)
* RoxiCake Gamer (YT_RoxiCake)
* Royale Dior (ItsRoyaleDiorYT)
* Ruby Games (ruby_rubeYT)
* RufflesOfficial (RufflesPlaysMC)
* Rupthy (Rupthy)
* RussoPlays (RussoTalks)
* S\_Viper (S\_Viper)
* SamHandling (DigitoPlaysThis)
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
* SimplyCoco (VURIIN)
* SkippyPlays (skipsk0p)
* SlEGHART (SlEGHART)
* Sleigher (Gsleigh)
* Slykage (Sly_Kage)
* Sniffycat (SnickerHoops)
* Snug Life (YT_SnugLife)
* Sofia Tube (sofiatube7)
* Solem Archive (iJackeryz)
* Sons Of Fun (SonsofFun_YT)
* Sonti (Sonti_cg)
* Sopo Squad Gaming (mikedrop937, CammyxBoba, and soposhadow)
* Soy Blue (SoyBluee)
* Soy Mandy Games (SoyMandy2020)
* SoyBlecus (Blecusito)
* SoyLuz (LuzCute24)
* Spekツ (ONESIEHOARDER308)
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
* ThatGuy (ThatGuyTG)
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
* TOP 10 Família Betero (BianoBetero)
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
* Vitamine (VitamineRoblox)
* Vitória MineBlox (vitoriamineblox11, anamineblox11, and JackieMineBlox11)
* Volt (v0_1t)
* WaffleTrades (WaffleTrades)
* Waike (JJ_Waike)
* WeirdBlox (WeirdBloxOfficial)
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
* Xpie (Xpie101gamer)
* Yammy (yammyxox)
* Yayixd Changuito 🐵 (YAYIXDCHANGU)
* Yode Plays (yode1)
* YoSoyLoki (YoSoyLokiYT)
* yTowak (yTowakGb)
* ZacharyZaxor (ZacharyZaxor)
* ZaiLetsPlay (YT_ZaiLetsPlay)
* Zaryee (JustZaryee)
* Zaze Blox (zazebloxx)
* ZeDarkAlien (ZeDarkAlien)
* ZephPlayz (StarCodeZephPlayz)
* Zerophyx (Zerophyx)
* Zilgon (Zilgon)
* ZoeTheNoob (z1oee)
* ZOMG (PandaBoss3_0)
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
* Lyna (Lynitaa)
  * Non-Giftcard Robux Giveaways
    * REGALO 100.000 ROBUX SI PIERDO ESTE RETO EN ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=ZNZLqUPWnfc
* SeeDeng (SeeDank)
  * Phishing
    * PLAYING ON A FAN'S ACCOUNT IN ROBLOX (SPENDING ALL THEIR ROBUX)
      * Logs into the account of a fan. Mentions a lot of other people sent their username and passwords.
      * URL: https://www.youtube.com/watch?v=9qOAqB6E-z8
* Smurfzin (Smurfzineo_YT)
  * Information Collection
    * O Fim da Black Friday no ROBLOX..😔❌
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=T3_-itX5bZo
    * 😡O DONO DO CORRIDOR OF HELL ME BANlU?? (Desafiei Ele)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=W2Et7HKrgTI
    * COMO FAZER UM DOMINUS DE POBRE no ROBLOX KKKK😂
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=BOjUJ1jbqes
    * SOFRI R4ClSM0 NO ROBLOX..😔
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=8nSTEw92zv0
    * AVATAR DO AMONG US, SÓ QUE DE POBRE KKKKK 😂
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=SXGypXk_Sak
    * Meu ROBLOX virou uma LOLI.. '-'
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=vj1DhfrndlY
    * 5 Jogos que FALIRAM do Roblox..😢
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=qML0FYauuVM
    * Esse foi o PIOR Evento do ROBLOX??..😔🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=95xzYtGUxcQ
    * AVATAR DE GRAÇA!! DO LIL NAS X do ROBLOX (COMO??)🤠💰
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=IRnm4G7uoAc
    * O Triste FIM do OOF...😔
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=O8-tqmKoO88
    * 😨A HISTÓRIA DA DEATH DOLLIE A ''BONECA ESTRANHA'' do ROBLOX..
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ySmw-oU0BGY
    * 🎅VAZOU!! ITENS GRÁTIS de NATAL do EVENTO LIL NAS X 🔥
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=nJMH_tb4nPM
    * 🤠TUDO SOBRE o NOVO EVENTO do LIL NAS X no ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=udQkylRRjUQ
    * 5 Rostos que foram BANlD0S do ROBLOX...😨
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=eeJ1uAJrvuY
    * DOMINUS, só que na Vida Real..🤔
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=iblP6zPfNxI
    * o Roblox está querendo ficar ''REALISTA''...🤔
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=V7S4eSzfbwI
    * Você realmente conhece o ROBLOX?? 🤔
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=3SrhamYvuA4
    * 😂AS PIORES CÓPIAS DE SHINOBI LIFE 2 KKKKKKKK
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=fEOjP9fSRIg
    * EU MOSTREI O ''ROSTO'' no Corridor of Hell...🤔
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=1MMPNemV6xI
    * 🔥CORRA!! LANÇOU a CAUDA de PAVÃO \*Item Grátis\* (Wintery Peacock Tail)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=U_BvcWsukvo
    * 5 Contas muito ESTRANHAS do Roblox..
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=YP8dojzOXQE
    * ❄️NOVO ITEM de PROMOCODE EM BREVE no ROBLOX!! (Wintery Peacock Tail)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=taCVdwxkERg
    * ITENS do ROBLOX, só que na Vida Real..🤔
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=xgVomppyw-M
    * 😂AS PIORES CÓPIAS de Tower Of Hell KKKKKKK
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=cgLoSaCM0_4
    * 😷5 Melhores Jogos do ROBLOX para Jogar na QUARENTENA🦠🎮
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Kg9NgEmiWiU
    * 🤔O Que fazer com APENAS 10 ROBUX no ROBLOX?? 💰💸
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=vtN0V9Sf0D8
    * 👧APENAS GAROTAS PODEM ENTRAR NESSE MAPA...
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=lY7xR3wqx6U
    * 🎃10 Itens de HALLOWEEN do ROBLOX que foram BANID0S...
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=cAxkIyZiCpY
    * 🦊🔥CORRA!! COMO CONSEGUIR o PROMOCODE GRÁTIS \*Flaming fox shoulder companion\* 🔥
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=H54KAolf8dg
    * 🔥COMO CONSEGUIR A NOVA ASA GRÁTIS ROBLOX!! (Topaz hummingbird wings)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=wNi7SYNWmyY
    * 😡ESSE JOGO FOI BANID0 POR COPIAR O ADOPT ME!! ❌
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=mwwZC4hdCpA
    * 🐋🔥CORRA!! COMO CONSEGUIR O NOVO PROMOCODE GRÁTIS \*Dapper Narwhal\* 🔥
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=FQ1BPyLjUS0
    * ESTÃO TENTANDO REMOVER O ADOPT ME DO ROBLOX..
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=E6A6gIeGRGQ
    * É sério isso ROBLOX??...
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=i-57wjKoJ3I
    * 😡ELA ME AMEAÇ0U E TENTOU R0UBAR MEUS ITENS CAROS do ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ed_HbbXZBMQ
    * 🔥COMO PEGAR O NOVO ITEM GRÁTIS do DIA ESPIRITUAL 2020 (Shoulder bag of Spirit Day 2020)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=j41iNr5rams
    * 😲COMO FAZER O AVATAR do AMONG US no ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=XNUG5psYuKk
    * 🔥CORRA!! NOVO PROMO CODE GRÁTIS do ROBLOX ACABA de SAIR!! (Socialsaurus Flex)
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=0mXYtNwdwlc
    * 😲VAZOU!! NOVO PROMOCODE de 2 MILHÕES de SEGUIDORES (Socialsaurus Rex)
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=v3zQ2YpOhXc
    * 👻NOVA ASA GRÁTIS!! de PROMO CODE CHEGANDO NO ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=9gdlwuFok7c
    * 🎃VAZOU!! NOVOS POSSÍVEIS ITENS GRÁTIS de HALLOWEEN 2020
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=XEJuXpa5OWc
    * A HISTÓRIA DOS BACON HAIRS...(2014-2020) 🕊️
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=hvXnwip6m_4
    * AINDA VALE A PENA COMPRAR O CARRO VOADOR DEPOIS DE 2 MESES??🤔
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=kUqZfE5ySGw
    * 5 Itens do ROBLOX que NINGUÉM CONHECE!! ❌
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=DM2087RDP-Y
    * 🎃FAZENDO AVATARES RICOS DE HALLOWEEN no ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=pvijg0dIDgI
    * 🎃ITENS GRÁTIS de HALLOWEEN COM CARTÕES DO ROBLOX??
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=wGok2GL_WCI
    * PAREM!! de JOGAR ESSES MAPAS do ROBLOX❌ (VÃO SER BANID0S)
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=Fbs7st-yExA
    * 🔥NOVO ITEM de CÓDIGO GRÁTIS!! EM BREVE no Roblox (Dapper Narwhal)
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=OcbcAC1yHsc
    * 😲VAI TER EVENTO DA AVA MAX DENOVO!! (VÃO DAR OS ITENS GRÁTIS??)
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=p0uR4g2Wwvc
    * O NOVO AMONG US EM 3D DO ROBLOX!!! 🔪😲
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=m_m2KC63Rmo
    * O ROBLOX ENGANOU TODO MUNDO?😞 (CADE OS ITENS AVA MAX??)
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=OE-zdU4qgTE
    * 🔥COMO CONSEGUIR TODOS OS ITENS GRÁTIS DO AVA MAX!!😍
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=Mh-VwqMPQGQ
    * 🔥TESTANDO O PRÓXIMO ITEM GRÁTIS do ROBLOX!! 🐱(Kitten Wizard)
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=lYUnrLp480k
    * QUEM QUE PODE TER ESSA NOVA CARTOLA??🤔🎩(Approved Top Hat)
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=ht73WR5mKb8
    * O FUNERAL DOS BACON HAIRS (sentimos sua falta)😔💔
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=0YDcphJAafQ
    * O GODENOT É MEU AMIGO NO ROBLOX??😲💖
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=12gxStPl7Ck
    * ELE FOI BANID0 POR FAZER RITUAIS NO ROBLOX..😨🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=S-5jZD53v8A
    * OS BACON HAIRS FORAM EXCLUÍDOS DO ROBLOX..😞🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=NDU0OQI0vaQ
    * COMPRANDO UM HOTEL DE LUXO!! \*Ficamos Ricos?\* 😍💰
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=QP3EwWCXVRM
    * OQUE FAZER COM APENAS 1 ROBUX No ROBLOX?? 💰🤔
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=BoNexsTUdcQ
    * O SIREN HEAD INVADIU A PIGGY?? \*inacreditável\* 🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=bdadn6qpneU
    * COMO QUE ESSE CARA FICOU SEM ROSTO??..❓🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=EPupOJy6Mvk
    * AS PIORES CÓPIAS DO SIREN HEAD!!..(ROBLOX) 📢🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ZOYK1pAHcVk
    * EU JÁ FUI AMIGO DA JULIA MINEGIRL...😲❌
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=CgYyrXYM6c4
    * 5 YOUTUBERS que o Roblox BANIU PRA SEMPRE...🚫🎥
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=iVSV_FWK-Rk
    * A HISTÓRIA DOS BIGHEADS \*FORAM PROIBIDOS?\* 🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=taGXjCwZfw8
    * COMO CONSEGUIR SEGUIDORES INFINITOS NO ROBLOX!! ✔️
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=1VXEKlaIdgM
    * SOFRI PREC0NCEIT0 POR SER DEFICIENTE NO ROBLOX..😪♿
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=GFWwrU3UkuY
    * VAI TER CHAT DE VOZ NO FALL GUYS?? 🌈🎤
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=txGqapnKqIg
    * 5 Itens que o Roblox BANIU PRA SEMPRE...🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ASc1eGRl2cw
    * É PR0IBID0 GAROTAS NESSE MAPA!! ♂ 🚹
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=F_u4kej8UIM
    * PAREM!! DE COMPRAR CAMISETAS NO ROBLOX 💸🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=vXzrt2wayxE
    * EU VIREI UMA E-GIRL NO ROBLOX...
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=vleWgt29lV0
    * AS PIORES CÓPIAS DE VALORANT!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=UrZKk-KVvU0
    * AS PIORES CÓPIAS DE PIGGY DO ROBLOX 🐷🔪
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=mHJvELEb9ek
    * SE A PIGGY ME MATAR O VÍDEO ACABA.. (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=o8_qOzk6chU
    * NÃO SENTE nessa cadeira no roblox...🚫🪑
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=1XEeRfWrRoU
    * nunca coma esse hamburguer do roblox...🍔🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=z3yzQ0E4cLI
    * JAMAIS CLIQUE NO BIG HEAD.... 🚫⚠️
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=3qNw_xC6s2c
    * JOGANDO ROBLOX NOS GRÁFICOS NO ULTRA!!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=v8RB7TBog_w
    * ANO NOVO NO ROBLOX!! FELIZ 2020 🎉 🎉🎇
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=4D5fgCI5xIk
    * O ROBLOX DESTRUIU O NATAL...🎅🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ThuzIZKRltw
    * CASA ASSOMBRADA DO PAPAI NOEL DO ROBLOX 🎅🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=vnxwV8TQejA
    * JAMAIS ENTRE NESSE ESGOTO DO ROBLOX...
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ogeqdkoy3wI
    * TROLLANDO COM HACK NO AMONG US
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=FzNVCDLvyGg
    * JOGANDO JAILBREAK PELA PRIMEIRA VEZ
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=LM5Gz52mECI
    * AMONG US DENTRO DO ROBLOX? 🤫🚫
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=Yd8rmhHJCAo
    * O IMPOSTOR ESTRATÉGICO 🤫 - Among us
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=T1pbDeEoRdw
    * AGORA SÃO 3 IMPOSTORES - Among Us 😱🔪
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=I-Z7zONdgjk
    * O IMPOSTOR SE DEU MAL!! - Among Us 😱
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=j2OIMd8Dw2k
    * HERÓI OU CRIMINOSO?? - Mad City Roblox 🕵
      * Description references the data collection website bloxawards.com.
      * Description references the data collection website bloxpoints.com.
      * URL: https://www.youtube.com/watch?v=9qysuoDwmRw
    * Como NÃO Sobreviver Em uma Pizzaria🐻
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=CPh9TwKtaVc
    * O JAILBREAK ESTÁ EM VERSÃO NATALINA!!?? 🎅
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=938cwxGp44Y
    * TODOS OS ITENS DA BLACK FRIDAY NO ROBLOX 😲
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=wHGhi-BfJ4Y
    * A INTERNET DE TODOS CAIU!! - MURDER MISTERY (Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=P4K3h8-V2tg
    * NUNCA PESQUISE ESSES NUMEROS NO ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=4k0RppuoevU
    * O EPISÓDIO MAIS ENGRAÇADO (Flee The Facility Roblox)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=brbtWcuQIFM
    * O ANTHRO R30 ESTÁ CHEGANDO NO JAILBREAK!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=0wiiB1HJesk
    * VAI TER NOVA ARMA NO JAILBREAK (CONFIRMADO)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=XpSnOeqSTQg
    * TESTANDO ARMAS COM O R11!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=CzAxp6ogzrA
    * COMO FICAR COM A CABEÇA INVISÍVEL NO ROBLOX (BUG)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=MX0ziPohjYQ
    * A VELHA ASSUSTADORA DO ROBLOX.....
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=L6MUlR6mdoM
    * O PRISON LIFE VAI SER EXCLUIDO? 😲
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=HKIeopW9hrA
    * VIDA DE YOUTUBER NO ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=uRwttmdyAmY
    * NOVOS CARROS REALISTAS DO ROBLOX?
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=CHMLjLeZNaI
    * ASSISTA ISSO OU PERDERÁ SUA CONTA...
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=dTJFmedqtIU
    * ROBLOX VS VIDA REAL
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=19y9Nd6XMr4
    * O FUNERAL DOS GUESTS NO ROBLOX....😢
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=QEgfrb0hd7E
    * O NOVO R11 DO ROBLOX!! (NOVA ANIMAÇÃO)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=LGwyUKURV9M
    * NUNCA ENTREM NESSE MAPA DO ROBLOX...\#2
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=yq_kskLLTG8
    * O NOVO ASA-DELTA DO JAILBREAK (ATUALIZAÇÃO)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=IVxMqs8TCBg
    * O ROBLOX PRECISA DE AJUDA...
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=ZN6LKmY98NA
    * JOGANDO A PRIMEIRA PRISÃO DO ROBLOX? 🤔
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=cQAek1vGIo8
    * NUNCA ENTRE NO MAPA DO LIL PUMP... (É SERIO)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=qY5S6_TmEgQ
    * O ITEM MAIS CARO DO ROBLOX 🎃
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=8SIf0551gx4
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

## Conclusion
The inconsistent enforcement of the Roblox terms of use and laws in the United States needs to be addressed to improve
community safety and improve the relations of the influencer program and developers. Robux giveaways and phishing can
give legitimacy to malicious people trying to get access to users' accounts while information collection puts the
information of minors at risk. Addressing these will make developers have more confidence in moderation and Roblox's
marketing team.