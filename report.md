# Roblox Influencer Program Unethical Videos
Generated May 1, 2025<br>
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
* Total videos: 997,530 videos
* Total videos found that match keywords: 57,291 videos
  * Total unprocessed videos: 11,271 videos
* Total videos found that are processed and marked: 570 videos 
  * Information Collection: 482 videos
  * Non-Giftcard Robux Giveaways: 87 videos
  * Phishing: 1 video

### No Videos Found
The following channels had nothing appear with manual searching. Videos may exist, but were not found.
* 39jeshi (39jeshi)
* 3SB Games (cakemiix and 3SBMiichael)
* Aati Plays (AatiPlays_Official)
* Abbaok (Abbaok)
* AbsintoJ (AbsintoJYT)
* Acenix (AcenixElMejorYoutube)
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
* Carmi Games (carmiYTgames)
* Carol TV (caaroltv)
* CatFer (SoyCatFer)
* CATYPINK PLAY (catypink15)
* cazum8 (cazum8)
* Ceddy (Ceddy)
* Cee\_berry (Cee\_berry)
* Chekecheto (Chekelete1)
* Cheo Power (cheopower)
* CherryAhrizona (baby_ahrizona)
* Chizeled (Chizeled_YT)
* Chocoblox (Chocoblox_93)
* Chrisandthemike (chrisatm)
* Christocream Roblox (Christocream)
* Chrisu (ChristsuYT)
* cKev (cKev)
* Claus (ClausOFC)
* Cleanse Beam (CleanseBeam)
* ComfySunday (ComfySunday)
* Complex Roblox (IamComplexRoblox)
* Conor3D (Conor3D)
* Cookie Cutter (CookieCutter)
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
* DuckXander (FoamyBubbIes)
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
* Família Betero TV (BianoBetero)
* Fanaticalight (Fanaticalight)
* FancySmash (RealFancySmash)
* faulty (Faulltyy)
* febatista (febatistaYT)
* fer999 (StarCode_fer)
* Fera (Fera)
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
* frenchrxses (frenchrxses)
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
* IntelPlayz (IamRoBuilder)
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
* Kitt Gaming (starcode_kitt)
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
* Muneeb (itsMuneeeb)
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
* PHMittens (PHMittensSTARCreator)
* Phoeberry (Phoeberry)
* PinkFate Games (PinkFateYT)
* PinkLeaf (RenLeaf)
* Play with me - Apps and Games (da\_dania and kaan\_xy2)
* Plech (PlechitoYT)
* Plique Games (PliqueYT)
* Poke (Pokediger1)
* PolarCub (PolarCub)
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
* Robuilds (Robuilds)
* Rod Velloso (viddrox)
* RODNY (RODNY_ROBLOX)
* Rovi23 (byRovi23 and MelLovesBunnys)
* roxhi roblox (12345roxyhi12345)
* RoxiCake Gamer (YT_RoxiCake)
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
* DefildPlays (DefildPlays)
  * Information Collection
    * MEGA FREE 60 GAMEPASSES SALE CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=iM_XrtQO_QU
    * NOOB INSTANTLY GETS ALL DEVIL FRUITS IN ANIME FIGHTING SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=0YtP4iPRWSc
    * All Free GENKAI SHISUI SHARINGAN Spin CODES! - Shinobi Life 2 Roblox \*FREE 1200 SPINS\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=WnZU04TXzbU
    * The MOST OP NOOB In Anime Fighting Simulator HISTORY! Roblox \#1
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=PDxv2eZ9bAA
    * ADMIN GAVE A SECRET CHAMPION LUCK CHIKARA CODE IN ANIME FIGHTING SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=cvQqnk_KmDY
    * ALL 21 YOUTUBER 100,000 CHIKARA CODES IN ANIME FIGHTING SIMULATOR
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=05bOC8qbEUk
    * ALL SECRET COSMETIC REWARD CODES IN ROBLOX IMPOSTOR!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=LhOX9AtZ-jQ
    * So I Opened 5,000,000 Eggs In Bubble Gum Simulator And Got This..
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Mv_J2efmKRw
    * 4 NEW SHINY ADMIN STREAM PET CODES IN Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=FesjCx3ecLY
    * HOW TO GET \*Free\* OWNER SWORD + SHADOW CLONE Skill In Anime Fighting Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=dF2Ixxhpxb4
    * SECRET FREE ADMIN Bubble Pass 15 CODES In Bubble Gum Simulator!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=hAwbWzPpgRs
    * I Hatched ALL SPLIT Pets And FREE 2000 Pet Gamepass In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=6WwH8ob2hVM
    * ALL NEW 2020 CHRISTMAS UPDATE PETS IN BUBBLE GUM SIMULATOR! \*SUPER COOL\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=dqh9E6rIBGU
    * NEW 650,000 CHIKARA TWITTER CODES IN ANIME FIGHTING SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=viWRjLOVhzk
    * NEW SHADOW EGG, LEVEL 60 POTION AND MORE IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=gEHveLSVCrw
    * FREE MAX ANNIVERSARY FIGHTING PASS 2 CODES In Anime Fighting Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=wwAZI9p9dJI
    * ALL NEW ARMAMENTS, MAXED FIGHTING PASS IN ANIME FIGHTING SIMULATOR UPDATE! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=xsDAP4dzJ7M
    * All SECRET Roblox Promo Item Codes! \*Free Roblox Avatar Items\* - November 2020
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=UcQ5ciNvJzQ
    * All 17 TWITTER ADMIN Chikara CODES In ANIME FIGHTING SIMULATOR Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=PSoX83PA7zg
    * HIDDEN OWNER HALLOWEEN OP PET CODES IN BUBBLE GUM SIMULATOR!
      * URL: https://www.youtube.com/watch?v=IWlYqN93igQ
    * SECRET 666,666 CHIKARA HALLOWEEN BATTLE IN ANIME FIGHTING SIMULATOR
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=WR2QVj307y0
    * ALL MEGA SECRET 900M EGG PETS IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=KWMpSVRcmJI
    * INSTANT FREE 900M EGG SECRET PETS IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=rXuhthEZGdM
    * I HATCHED 750,000 EGGS 900M SECRET PETS IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=vj7YGub31DQ
    * FREE SHINY 900M Cartoon Pet CODES In BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=UiuWtaGQ9sA
    * I HATCHED ALL 900M EGG PETS IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=XA8iffWlaWw
    * ALL NEW 900M EGG LEGENDARY PETS IN BUBBLE GUM SIMULATOR! Roblox \*LEAKS\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=oRn-S4EJejY
    * FREE 100,000 Chikara Champions In ANIME FIGHTING SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=wR7gXNXz9iQ
    * How Op Can I Get In 1 Hour As A NOOB In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=w_swNiqnjuk
    * Trick or Treating BUT I Give OP Pets In Bubble Gum Simulator Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=XAko1zBfsBs
    * Suprising NOOBS With OP Halloween Pets In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=P3DCSZkIH-c
    * 10X Secret Luck Event Challenge In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=gbM57o0ZjCQ
    * FREE 1 MILLION CHIKARA BLOODLINE BATTLE In Anime Fighting Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=c3dkA8aNfC0
    * FREE MEGA SHINY HALLOWEEN PETS AND SECRET OPENING IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=1Rmi1S1VGXA
    * FREE HALLOWEEN PETS AND SECRET OPENING IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=GCYT14fBtbc
    * All MAX SPEED Pet Codes In SPEED RUN SIMULATOR - Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=w06hybXnqDM
    * ALL October Free Roblox Promo Codes 2020 \*Free Cosmetic items\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=R-UadbXWU5I
    * All Secret FREE 666 Spins Codes In Roblox Shinobi Life 2
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=02-GMN3HU88
    * FREE HALLOWED SPIRIT Pet Update Codes In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=3ljfmzXAVd4
    * Free PREMIUM Halloween Bubble Pass Pets In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=iUzJFMAZ9lU
    * Spending 1,000,000 CHIKARA For New CHAMPIONS IN Anime Fighting Simulator! \*SUPER OP\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Ihyz9ub4X7c
    * Noob With HALLOWEEN SECRET To PRO In Bubble Gum Simulator! \*SUPER BROKEN\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ULDAa8WQ56o
    * New Secret Steampunk Pet Update Codes In Tapping Simulator! \*SUPER OP\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=VpIbFNwrgto
    * 15 FREE Bloodlines Halloween Update Codes In Anime Fighting Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ZVdbb6PpvYw
    * All MAXED Halloween Event Pets In Bubble Gum Simulator!! \*Giveaway\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=lew8Ylg-R2c
    * FREE Halloween Moon Pet & Candy Codes In Saber Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=b3b2o25FVjY
    * I Got ALL HALLOWEEN Pet Codes In Bubble Gum Simulator!! \*Max Reward Pet\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ADSvWmmVGaI
    * FREE Tormentor Pet, 2X Treats In Bubble Gum Simulator Halloween Update! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=slJ9ywLc_pE
    * Secret UNDEAD CRYPT Boost Codes In Tapping Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=7Ci7pI_A7vI
    * GOLDEN Crown Codes, Vent BUFFS And MINIGAMES In Impostor Update! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=YE8cuGOQYQc
    * Insane 700 IQ Door Sabotage Plays In Roblox Impostor!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Entw4tLKhfg
    * All 135 Free Gamepass Codes In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=VUV6IvntsOA
    * I Pulled Off A 600 IQ Impostor Play In Roblox Impostor!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=eeKtYa1aDLo
    * Roblox Impostor Hide And Seek! \*New Gamemode\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=oVNG6j_aUyg
    * Winning With 500 IQ Vent Plays in Roblox Impostor...! - Roblox Among Us
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=SMXPAJAzRu4
    * Everytime I KILL, I Can BREATHE In Roblox Impostor! \*Mr Beast Challenge!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=vC6yqVcoLiA
    * SECRET \*INSTANT WIN\* Hat CODES In Roblox Impostor! (Roblox Among Us)
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ZA-IWmWXtVg
    * INSANE 100% Winrate SECRETS In Roblox Impostor! (Roblox Among Us)
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=e3AQV-z9s2o
    * AMONG US But Its In ROBLOX! Roblox Impostor
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=h7DgqM-CUSw
    * 3 Secret FREE GOLDEN HAMMER Codes! Mining Champions Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=d89QtsECjgQ
    * How To Get \*NEW\* COPPER And STEEL In Roblox Islands! \*FACTORY UPDATE\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=jRp-bhYr15k
    * RANK 1 MAX DMG IN NEW BOSS DUNGEON UPDATE!! Roblox Tapping Simulator
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=i5ufKb_I_mk
    * NEW "LEAKED" Industrial Update In Roblox Islands! \*New Machines, Drills And more!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=SD6o27h62vE
    * ALL 8 Roblox Mining Champions Codes! \*FREE AUTO MINE/SELL!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=7bz8w9exbPM
    * EVERY Fall Event Egg Pets In Bubble Gum Simulator! \*SUPER OP\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=kVVCG3gQfZU
    * 35 OWNER 1 YEAR VOID CHARM CODES IN SABER SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=qAUxn_x46rA
    * FREE Maxed FALL EVENT Rewards & Secret Hat In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=tjxn7gI2Ufk
    * MAXED Fall Event PREMIUM Bubble Pass Codes In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=vEEOE3x3Ej4
    * I Went From NOOB To MASTER In Roblox Cosmic Heroes \*DEFEATED OWNER\* W/ @MrWoofless
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=C1ma5g7wtMU
    * ALL Secret DESERT PET Gem Codes In Tapping Simulator Update! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=8TJZxq7t4W0
    * How To Get EVERY Secret SKIN In Arsenal! \*Beelzebub, Manic, Bigfoot and more!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=V_XzUKz8t9A
    * ALL 10 WORKING ROBLOX PROMO ITEM CODES! \*Free Cosmetics!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=FS-nAPwyzOo
    * RAINBOW FIREFLIES IN ROBLOX ISLANDS SKYBLOCK! \*Best Way To Get Them\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=-jmTjn9TBqA
    * New Secret Obby World GLITCH \*Instant Finish\* In Tapping Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=oWQ3nNiliu0
    * Secret SHINY PREMIUM Pet Codes In Pet Battle Simulator! \*Instant Best Player!\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=tsDPf7BhJxA
    * 21 SECRET OP EGG UPDATE CODES IN TAPPING LEGENDS! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=oIDqlIofPt0
    * All MEGA BOOST 60M Codes In Tapping Simulator \*FREE 10X BOOSTS!\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=WvpcW62zX-M
    * INFINITE \*AFK\* Crystallized Iron Farm \*9M $$$ A HOUR\* In Roblox Islands Skyblock
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=VMkJObVTiM0
    * MAXED Premium Fighting Pass Codes In Anime Fighting Simulator!! \*GET IT FREE\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=lsNZUNQ307c
    * ALL BEE Update SECRETS You MUST Know In Roblox Islands Skyblock \*SUPER OP\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=XQ3FvYOrtCI
    * All NEW CITRUS OVERLORD Pet Update Codes In BUBBLE GUM SIMULATOR! Roblox \*GET FREE!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=dM-5wb4VpXw
    * FREE Diamond Play Button, YouTubers And More In MY RESTAURANT UPDATE! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Gqy259B4UUQ
    * Roblox Pet Simulator The Best Forgotten Game.. Codes + More!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=pERWNSbgAdU
    * Rainbow ROBUX Gear & Pets BREAK Tapping Simulator Forever... \*NOT GOOD\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=5yjQ0uWsHv0
    * NOOB To MAX WAR GENERAL In War Simulator! Roblox \*30 MINUTES ONLY!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=8O2RW5jWlRA
    * 25M VIP BOOST Update Codes In Tapping Legends! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=wGgw3L97Dq8
    * How To Get 3 MILLION Coins In Roblox Skyblock Islands FAST! (NOOB FRIENDLY) \*IN 1 HOUR\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=YDVHEDBAV0Y
    * Infinite POWER Chest Item Glitch In Tapping Simulator! Roblox \*FREE RAINBOW GEAR\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=OFsloxTqwo8
    * FREE INSTANT BILLIONS, Pirate Customers & Limiteds In My Restaurant Roblox!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=CcdeEOtMI3M
    * All 50M PHOENIX UPDATE Codes In Tapping Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=nuY55l0kbsk
    * Owner Added MY Update In Roblox Islands Skyblock!? \*BEST UPDATE!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=LXNGrl4NGgc
    * Hatched The \*BEST\* 1/15,000,000 Pet In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=wqBen6TAX20
    * Roblox Arsenal BUT If You WIN You Get $10,000 Robux
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Oyr9Ww8erRY
    * ALL 27 SECRET 1 BILLION CODES In Bee Swarm Simulator!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=gZzBLRBR7S0
    * SHINY Season 12 Bubble Pass Codes In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=kJObC0GhxdI
    * 5 BILLION $$$ Limited GLOBAL Trading Update In MY RESTAURANT Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=p1tz6exps6o
    * Epic $10,000 Robux Anime Fighting Simulator Challenge! (Roblox)
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=qJ6ytSPToCw
    * SECRET Lost World SUPER GEMS CODES In Tapping Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=RWn7ZaXEy4Q
    * Unlimited SUPER REBIRTH TOKEN PETS World In Tapping Simulator Update! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=FmAiR6uoyGU
    * Roblox Skyblock But The Best Island Wins $10,000 Robux
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=v2fAt7MfHLQ
    * BEST WAY To Get A FREE Secret VAMP BOW In Skyblock Islands - Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=9tdH7jCt0MQ
    * ARTIFACT BOW, POTIONS, SCORPIONS In Skyblock Islands Update - Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=6McHbtJuJCw
    * Roblox Skyblock But This Item Is Worth $10,000 Robux
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=e_gmhpz4U7k
    * FREE 2X CLICK SUPERNOVA PETS UPDATE In Tapping Legends! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=n7fgw2DuITg
    * All 23 SECRET FREE 10M PET CODES In Tapping Legends! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=h3VKtSIQKws
    * SECRET GEAR 40M UPDATE CODES In Tapping Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=m3atU6vFpF0
    * BACK 2 BACK SECRET PETS IN Bubble Gum Simulator!? \*FREE OVERLORD PLUSHIE\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=2OiZDR4j01s
    * NEW POPULAR FREE JUKEBOX ISLANDS UPDATE! Roblox Islands / Skyblock
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=NqvHYALcClM
    * SECRET ELEMENT PETS AND FREE POWER BOOST IN TAPPING SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=mBPXLDDN0Sw
    * MAXED CARNIVAL CHALLENGE CODES IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=vt1_sO224r4
    * SECRET PET OPENING + BASILISK PET GIVEAWAY In Roblox Bubble Gum Simulator!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=00WWVt98Z5o
    * ALL 57 FREE Secret Basilisk Pet Codes In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ObVb8rPhH-g
    * 11 SECRET WILD WEST Super Gem Codes In Tapping Simulator! \*BEST PET\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=86abQKWkjn8
    * Free KING CRIMSON Stand & MAGMA FRUIT Chikara Codes In Anime Fighting Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=OY94K_PNSjo
    * BUYING THE 1,000,000,000,000 CARNIVAL LOL PET IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=HBIzfqagfl4
    * EASY WAY To get a FREE SECRET BLUEPRINT WEAPON In Roblox Skyblock / Islands Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=CiTMvJclqNY
    * Free SHINY Carnival PET Codes In Bubble Gum Simulator! \*SHINY SHADOW HUNTER\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=GVzoUl0b2IQ
    * 9 SECRET Permanent HELL WORLD PET Codes In Tapping Simulator! \*NEW SECRET PETS\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=CnkKjYgvGvU
    * How To Get INSTANT FREE Spellbooks In Roblox Skyblock / Islands
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=KQpnyXyvl1g
    * Getting All Secret Pets In The HEAVEN EGG In Tapping Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=TgTbGiK9uiI
    * SECRET 30 Million Secret Pet Codes In Tapping Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=NrY39GQ5CQE
    * Mega 2X Event Opening For 24 Hours And Got THIS SECRET😱😱!! Bubbble Gum Simulator
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=5_VMNhWIpEY
    * FREE MAX RANK And SHADOWSTORM Pets In Ninja Legends! \*SUPER BROKEN\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=1XTIZRZ5HD0
    * FREE New Slot Machine + 3X Money GOLD Food In My Restaurant Update! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=PRW9bRVwaRc
    * 2X POWER Codes And BEST WORLD Unlocked In Tapping Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=LzzqZ03dLok
    * Free BEST SECRET Mystic Gem Pet In Clicking Legends! \*SUPER BROKEN\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=zH4xq_LIHJE
    * New FREE SKYBLADE LEGENDS PACK CODES In Ninja Legends! \*SECRET PET CRYSTAL\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=VyGbKPmQ5EY
    * ALL 10 Secret GOD PET Codes in Clicking Legends! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=I7oJVdVc4vQ
    * My Restaurant Makes 750 MILLION $$$ A Day!! \*My Restaurant Best Setup!\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=EC6HW6IMi8Q
    * Super Rebirth SECRET PET Update CODES In Tapping Simulator! \*SUPER OP\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ZbcjXEEfoeE
    * MEGA BOOST And FREE Season 11 Bubble Pass Codes In Bubble Gum Simulator! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=jVxBKIt4PD4
    * NEW STARFRUIT SEEDS, BAMBOO, SWORDS In Roblox Skyblock! Skyblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=ApfD7qykuF4
    * FREE RESTAURANT THEMES, BOOST WISHING WELLS In My Restaurant Update! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=JpbvSBDSeG4
    * How To Super Rebirth Every 10 Seconds In Tapping Simulator! \*BUYING MAX EVERYTHING\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=yDyqpLJCqJw
    * SECRET PETS + NEW Spawn UPDATE In Tapping Simulator!! \*SUPER BROKEN 😱\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=NpmIYEeLPL8
    * All Hidden SECRET TIPS You SHOULD Know in Roblox Skyblock / Skyblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=wljQzTYd-g4
    * SECRET 24 Hour Life Hack Boost Codes In Clicking Legends! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=VWr5ypqBhn8
    * 2X SUPER REBIRTH Tokens Codes + MASTER PETS In Tapping Simulator!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=AmagYuPOx2E
    * 96X SECRET PET Hatch Chance CODES In Bubble Gum Simulator! \*SUPER BROKEN\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=pAQ09lIp3u8
    * FREE Firework Gamepass + MORE Blocks Update In Roblox Skyblock / Skyblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=mD4LvEFkAUE
    * UNLIMITED RICH Celebrities And Luxury Furniture In My Restaurant! \*ULTRA FAST MONEY!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=a0b6z9UIwgA
    * SECRET 8Bit OMEGA Pet Gem Codes In TAPPING SIMULATOR! \*130 QA Gems\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=aOq9UPXYiQ4
    * INFINITE VIP CUSTOMERS With FREE Royal Bundle In My Restaurant! \*NEW ??? SPAWN SHRINE\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=tOhyP9bnbj0
    * All 5 SUPER REBIRTH Codes in Tapping Simulator! \*Super Fast Rebirths!\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=-YaKrRVL_1Y
    * I Maxed EVERYTHING In My Restaurant + Secret ??? BADGE In My Restaurant! \*MILLIONS OF $$$\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=NQUx1s2jG_o
    * Hatched 800m Egg For 24 Hours And I Got These Pets! - Bubble Gum Simulator
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Vz_73XjwI00
    * Easy Way For AFK CRYSTALIZED GOLD in Roblox Skyblock! Skyblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Yfgxfe_mWAE
    * I Got RANK 1 In My Restaurant Simulator + FREE MONEY TREES! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=vOqc9H1I9uk
    * 100 SECRET PET EGG CHALLENGE IN BUBBLE GUM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=hWOIvgCR1tU
    * I Got New OMEGA Pizza Pets In Tapping Simulator! 600QI Taps! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=0Exl1rNKBUU
    * Secret 10,000 ROBUX FRUIT In Anime Fighting Simulator! - Challenge
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=c7Tfyv27Zn8
    * I Had 1 hour To Trade A Kitty In Bubble Gum Simulator.. AND GOT THESE PETS!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=LgU12BtFlPw
    * HOW TO GET EVERY SECRET PET IN BUBBLE GUM SIMULATOR 2020! \*SUPER OP\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=KgBZ7tkOf60
    * NEW OMEGA SPACE PET TAPPING SIMULATOR CODES! \*3.31 SP TAPS!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=da2BGKwLqGo
    * All 25 ULTIMATE SECRET HAT Codes In Bomb Simulator!! \*FREE 45X COIN/GEMS\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=z70EIGC-XPI
    * ALL 9 FREE SWORD STYLE CHIKARA CODES IN ANIME FIGHTING SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=VyT2flz8xcw
    * NEW DOMINUS QUEST EVENT CODES IN BOMB SIMULATOR! Roblox \*LIVE\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=PtLHOpcNs9o
    * NEW Washing Station CHANGED AUTOFARMS FOREVER.. In Roblox Skyblock \*NEW BEST FARM\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=KnrBUI8UEb8
    * ALL 20 MYTHICAL GEM PET CODES IN FISHING SIMULATOR! \*MAGIC MYTHICAL PETS\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=hyC1oU5QVYg
    * Hatched SHINY LEVEL 50 Galaxium! + FREE Season 10 Pass - Bubble Gum Simulator
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=cgP96KvAUrs
    * SECRET SHADOW REALM POTION CODES IN BUBBLE GUM SIMULATOR UPDATE! \*How To Enter\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=65sfVBUJ5Mw
    * BUYING 192T CYBER SWORD AND 3T GEM CODES IN BOMB SIMULATOR! ROBLOX
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=5XuQPy3xpCo
    * 25 FREE MYTHIC Jumbo June Pack CODES IN BEE SWARM SIMULATOR! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=VA0sHpm4eCQ
    * NOOB To MASTER In Slamming Simulator \*SECRET OP TRAINING\* + CODES Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=vYdSNDWhoIc
    * New $10,000,000 Lottery Vending Machine In Roblox Skyblock! \*INSANE MONEY\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=d4Rk-f8rFGg
    * I Completed The SHINY Mythic Forest Index In Bubble Gum Simulator And THIS HAPPENED!?
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=BM9t4a3tYKk
    * NOOB To MASTER IN BOMB SIMULATOR With OWNER COIN CODES! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=S8O3PsEAtXo
    * This Update Could END Bubble Gum Simulator 😭 \*GIANT METEOR\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=xgpinLX9xr8
    * 😔Noob VS 😎Pro VS 😈HACKERS In ROBLOX SKYBLOCK! \*Funny\*✨
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=sFXytKCo3l0
    * ALL 14 SECRET CHIKARA CHAMPION CODES IN ANIME FIGHTING SIMULATOR! \*INSTANT BEST KAGUNE!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=FvTTfKn8NeU
    * SECRET ADMIN ITEM SPAWN TOTEM In Roblox Skyblock! \*HOW TO GET ONE\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=wGjqcmA6F0Q
    * I Made INFINITE VENDING MACHINES But EVERYTHING Is FREE IN Roblox Skyblock!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=umx_A5biOww
    * Bought The HIDDEN SECRET PET For 750 BILLION Flowers!! 2X Flower Event In Bubble Gum Simulator!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=-3bNIwyN1-0
    * SECRET 500,000 CARROT CHALLENGE IN ROBLOX SKYBLOCK \*FREE 12 MILLION $$$\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=wrXBODitim8
    * Went From Noob To LEGEND In Clicking Simulator 2.0! Earned 10 BILLION Rebirths!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=vSEiWryBdlg
    * I Hired YOUTUBERS Builders For $5,000,000 And THIS Happened In Sky Block Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=W5oD0tj4hZs
    * NEW POTATOES, BUILD BLOCKS AND MORE IN ROBLOX SKYBLOCK UPDATE! \*NEW MEGAFARMS\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=vm-hMkCZB7s
    * TRADING, NEW TOTEMS AND SLIMES IN SKYBLOCK UPDATE SOON?! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Blsv1XGpqSA
    * How To Get FREE MAXED MYTHIC SPRING PETS In Bubble Gum Simulator!! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=YIXOfzOmgFc
    * How To Get EVERY Vine Egg Pet For FREE!! In Bubble Gum Simulator Roblox \*SUPER OP\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=GJYHHsMH7mo
    * All \*FREE\* SPRING DRAGON REWARD PET Codes In Bubble gum simulator update! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=UneGbjznEM4
    * I Let Fans Do ANYTHING To My Roblox Skyblock Island... \*BAD IDEA!?\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=9uzCeNtysGM
    * Making A \*GLITCHED\* Glass Onion Farm In Roblox Skyblock \*MASSIVE BUG\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Am1IDZ7i3tg
    * ALL 37 \*FINAL\* Update Codes in Ninja Legends \*RIP\* Roblox Ninja Legends
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=2hhNqMcQGGE
    * HOW TO GET \*FREE\* GILDED GOD TOOLS IN ROBLOX SKYBLOCK!!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=XU_46hHluL8
    * Opening 100,000 Blossom Eggs And Got SECRET PET In Bubble Gum Simulator!!! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=JKEtDrhhssk
    * NEW GOLD \*GOD\* TOOLS AND BLOCKS IN ROBLOX SKYBLOCK UPDATE \*SUPER EXPENSIVE!!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=NOZsZi7szNM
    * How To Get A FREE SHINY THE KEEPER In Bubble Gum Simulator Spring Update! \*Giveaway\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=1xs1AVkCjGw
    * FREE SEASON 9 SHINY BUBBLE PASS PET CODES IN BUBBLE GUM SIMULATOR! \*FREE SPRING PETS\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=xaw1PeEFD2s
    * SECRET WAY To Get 1000 FREE Berry Seeds In Roblox Skyblock! \*INSANE METHOD!\*
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=nqX0ErySgn4
    * SECRET 1 MILLION $$$ BUILD QUEST IN SKYBLOCK!? Roblox Skyblock
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Yc2_DE7Bt98
    * I Made MAX HIGHT \*BROKEN\* AUTOFARM In Roblox Skyblock (Free MILLIONS Daily!)
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=_gwkqWuz-W4
    * I Made A MULTI MILLION $$ SKYBLOCK ISLAND \*INSTANT RICH\* Roblox SkyBlock
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=IDmKXijJL2Q
    * 7 FREE Shiny MYTHICAL Pet Codes In Bubble Gum Simulator Update! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=3cazQpaRjrs
    * The RICHEST ADOPT ME ACCOUNT! \*BUYING 1,000,000 BUCKS!\* | Roblox Adopt Me Gameplay
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=_Xs__xxpYks
    * The RICHEST ADOPT ME ACCOUNT! \*Buying 1,000,000 BUCKS!\* Roblox Adopt Me Gameplay
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=v92Ce1CAeoM
    * Trading This SECRET LEGENDARY ITEM Got me NEW LEGENDARY PETS! Adopt Me Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=fnS2tNkCyCw
    * We Found A HIDDEN EXTRA Scoob! QUEST in Adopt Me! \*FREE MEGA NEON?!\* Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=EY8WZcMirvo
    * Pet REWORK & RIP Twitter Codes In Adopt Me!? Roblox Adopt Me
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=Egsd0Aj7d8U
    * Completing The New SCOOB! Quest In Adopt Me Update! Roblox Adopt Me!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=D0WoOb03a8M
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
    * 🙈 BLIND TRADING My SECRET FROST SENTINEL Pet In ROBLOX BUBBLE GUM SIMULATOR!
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=YZcqa6l1Xow
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
    * 🍀 OPENING SECRET PORTAL UPDATE PETS FOR 24 HOURS IN BUBBLE GUM SIMULATOR! \*AND I GOT THIS\*Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=H9eAgOCDVNQ
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
    * MOST OP SECRET SPOT IN ROBLOX FORTNITE ISLAND ROYALE! \*You Have To Try This!!\* /W Rexex
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=5YkHehUrQEw
    * BUYING THE GOLDEN NUKE AND EXPLODING THE WHOLE PRIVATE ISLAND In Roblox Treasure Hunt Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=aHf1K-gDi_Q
    * (Codes) NEW REALMS, LEGENDARY PETS, MYTHICAL SCYTHE IN Roblox Mining Simulator \*THIS IS INSANE\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=5iPRaEubjBQ
    * \*NEW\* SNIPER ONLY CHALLENGE VICTORY in Roblox Fortnite: Battle Royale! - Island Royale
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=a-tS2VgnTLs
    * Buying PEGASUS Pet and Making $166.000 EACH SECOND In Treasure Hunt Simulator! \*BEST MONEY MAKER\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=V9AY9LIq1gw
    * NEW EARTHERN REALM AND LEGENDARY PETS! \*Make MILLIONS EACH MINUTE!?\* Roblox Treasure Hunt Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=RGY6qz4g0x0
    * SEARCHING THE BIGGEST GOLDMINE IN Roblox Booga Booga! \*Epic\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=0jYWSozXQhg
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
    * EXPLORING TOMBS FULL OF TREASURE IN ROBLOX EXPLORER SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=fQSCbMGrDBo
    * (WUT?) THE WEIRDEST SIMULATOR IN ROBLOX!? \*My Viewers Suggested This!\* - Roblox Lawn Mower Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qFQ7YbCKTY8
    * (Code) ALL NEW POSSIBLE TOOL SKINS AND PETS UPDATE IN TREASURE HUNT SIMULATOR!? \*What Do You Want?\*
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=l6SXdBPDGeY
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
    * HOW OP IS THE LEGENDARY INFINITE BACKPACK!? \*$100.000.000 IN 1 GO!\* - Roblox Treasure Hunt Simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=fXhBAYW7GJg
    * NEW BINOCULARS, MCLAREN SUPER SPEED AND BOX GLITCH UPDATE! - Roblox Jailbreak
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=JFFSz2gX7yw
    * (Code) ALL UP TO DATE 2018 CODES IN ROBLOX TREASURE HUNT SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=QXeVLJBn4eo
    * (Code) NEW UNDERWORLD, GOLDEN SPOON AND PRIVATE TREASURE ISLANDS UPDATE IN TREASURE HUNT SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=nFWmIJ3d9Iw
    * (Code) \*RAGE\* BACON HAIR STEALS MY SPECIAL LEGENDARY TREASURE CHEST IN TREASURE HUNT SIMULATOR!!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=xJenMD8aSnM
    * (Code) GETTING THE INSANE LEGENDARY TOOL IN ROBLOX TREASURE HUNTER SIMULATOR!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jbgIIXTw2Ns
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
    * HOW TO GET BOUND (MINI) HOOPA In Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=wWJKbrx4Yqw
    * New INFINITE ICE BACKPACK, ICE SPEAR And 2 NEW PETS In Roblox Snow Shoveling Simulator!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=bgqZBitqXrM
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
    * PREPARING FOR THE 8TH STEEL GYM AND MAGEARNA UPDATE! - POKEMON BRICK BRONZE - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=n3mlQk9aq5I
    * Roblox PERM BANNED LandonRB! \#RobloxWatch MEME NOT ALLOWED And Tacticles Reaches 100K!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=-8f3_HGkun8
    * (Code) FASTEST WAY HOW TO GET ICE IN THE NEW SNOW SHOVELING SIMULATOR UPDATE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=iAFtFREfKEw
    * (NEW) ICE MOUNTAIN EXPANSION AND LIGHTSABER?! - Snow Shoveling Simulator - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=PjedKC2tDcQ
    * (NEW UPDATE) HOW TO SPAWN THE ICE KING IN SNOW SHOVELING SIMULATOR - ROBLOX
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=f2iQhZBxzuY
    * PREPARING FOR THE NEW 8TH GYM AND NEW POKEMON UPDATE! - Pokemon Brick Bronze
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ZBV6ZupkSKM
    * NEW 8TH GYM AND NEW POKEMON UPDATE LEAKS! - Pokemon Brick Bronze
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=50E0rqWBXwU
    * \#DUCKSQUAD PERMANENTLY BANNED \#RobloxAlert FREE ROBUX IS FORBIDDEN?, NEW ROBLOX DEVELOPER LOGO!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=BVxJSyVqe1s
    * INVISIBLE SNOW COP TROLLING IN JAILBREAK! \*THEY CAN'T SEE ME\* - Roblox
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=l3lGoPtHotA
    * SHINY LEGENDARY GIVEAWAY AND PREPARING FOR 8TH GYM UPDATE IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=QJDXvgL36bo
    * \*NEW\* OMNI ENCHANCED CANNONBOLT VS OMNI ENHANCED GREY MATTER! - Roblox Ben 10 Fighting Game
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=pq_afTSN2co
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
    * My 2017 RECAP + PLANS FOR 2018! \#PinkArmy / DefildPlays
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=9Q4bC4y0eMk
    * BACON HAIR FINDS SECRET CHRISTMAS LEGENDARY In Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=HXMzG0M1fKw
    * BEN 10 GETS A SHINY CHRISTMAS SCEPTILE!? - Roblox Pokemon Brick Bronze
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=YQOJf3c_gOo
    * \*OMG\* How to FLYING TRAIN GLITCH in ROBLOX JAILBREAK!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=mHiGkaqWlmQ
    * \*OMG\* We Get A NEW OMNITRIX? BIGGEST MYSTERY In Ben 10 Arrival Of Aliens!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=un73qyVMCJ0
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
    * GETTING MEGA CHRISTMAS SCEPTILE!? \*WINTER EVENT UPDATE\* (Pokemon Brick Bronze)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=-9i6PHeh968
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
    * \*Insane\* STRONGEST Alien In BEN 10? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Es19-QZ5QHg
    * \*FREE\* HOW TO GET THIS INSANE FREE WINTER PRESENT From ROBLOX!? (Dominus, Robux or More?!)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=mc0Q2Rq5GPE
    * \*OMG\* Kevin 11.000 VS ULTIMATE ALIENS IN Ben 10! (Ben 10 Roblox)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ym91p_gLGrk
    * \*New\* All BEN 10 ULTIMATE Aliens VS ULTIMATE Aliens! (Roblox Ben 10) - Charity \#11
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=BRcro4nSmyM
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
    * \*OMG\* BEN 10 Vs FANS! \[SO MANY EPIC BATTLES\] (Ben 10 Arrival Of Aliens) 31 Days Of Ben 10 \#7
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=iJVs0Z38PPQ
    * \[NEW SERIES\] OMG THAT SCARED ME SO MUCH! | Underrated Roblox Games \#1 \[Roblox Alone\]
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=-SRX9H6KclE
    * \*INSANE\* The MOST OP ALIENS In Ben 10 Challenge! (Ben 10 Arrival Of Aliens) - Ben 10 For Month \#6
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=eEgcTz9yoPQ
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
    * INSANE LEGENDARIES! Randomizer NUZLOCKE! (Pokemon Brick Bronze) \#5
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=d68UiUa-KIk
    * \*INSANE\* WAY BIG Vs GREY MATTER!! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=QR4wZQCgWnw
    * \*Epic\* New OMNITRIX Random Alien GLITCH!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=3D6eJU9VPfw
    * WE BOUGHT OUR NEW HOUSE!! BUT THEN THIS HAPPENED! (Roblox Roleplay)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=IdR3wAHULRc
    * \*Epic\* Insane WAY BIG vs XLR8 Alien Battle! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=YCXaq04Yp0M
    * \*OMG\* These EPIC Aliens! VS Ben 10?! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Iabc1OBa8Js
    * \*New\* Insane Superhero JUSTICE LEAGUE in ROBLOX! \*Get Superpowers!\* (Injustice Online Adventure)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=fyt0Szi87Jc
    * \*Omg\* NEW Epic Ben 10 CLONING machine VS ALBEDO! (Ben 10 Movie Special)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=hRjDmD68l9M
    * \*Omg\* NEW Random OMNITRIX Transforms In These ALIENS!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=jlLr5NMfEwM
    * \*INSANE\* NEW Aliens In Ben 10! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=PeueHloQDKY
    * \*INSANE\* CANNONBOLT EVOLUTION Of Aliens! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=nBX4bfNb4UI
    * LEGENDARY STARTER!? Randomizer NUZLOCKE Challenge! (Pokemon Brick Bronze) \#1
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=1cyI_em_a7A
    * \*Insane\* New Aliens RATH And ATOMIX And Future UPDATES!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=ZobGnN2d3nw
    * \*Omg\* How to be INVISIBLE / Go THROUGH Walls EVERY ALIEN!? (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=mrAI2IB3Q0k
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
    * Bacon Hair Finds HIDDEN SECRET LEGENDARY Teleport In Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=z-t5EJ321YY
    * \*NEW\* ALIENS And Abilities Revamp UPDATE! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=2XlDuRz0WZs
    * \*Epic\* NEW Ben 10 REVAMP ALIEN Battles! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=7D2_hGWN2UI
    * \*Live\* Getting SHINY Lugia in Pokemon Brick Bronze!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=OYJf_JkbDyE
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
    * \*NEW\* ALIENS and EPIC NEW ABILITIES \[Big Chill, Anubis, Feedback\] (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=Q2JFZM2J-jA
    * BEN 10 uses ADMIN COMMANDS to TROLL IN ROBLOX!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=3BvQzIG2NcY
    * WARZONE BOSSES, EPIC CRATES AND MORE! | MINECRAFT SKYBLOCK X \#4 | (ArcadianMc)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=fUWqXkyx23o
    * BEN 10 VS BEN 23 IN ROBLOX!! (Ben 10 Arrival Of Aliens) /w ItsBear
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=3Tw9UXQKnCc
    * SECRET NEVER SEEN HALLOWEEN ALIEN ADDED!! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=frqqDImcYFk
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
    * HOW TO GET A FREE SHINY MIMIKYU IN POKEMON BRICK BRONZE!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=LJDnkRQr5Ok
    * THE FLASH VS ZOOM IN ROBLOX (Roblox The Flash)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=uFBDKnf7hCA
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
    * BEN 10 AND GWEN 10 VS CHARMCASTER IN ROBLOX \[INSANE\] (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=HibBUczgG4o
    * BEN 10 VS NEGA BEN 10 IN ROBLOX! (Ben 10 Arrival Of Aliens)
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=qOmRO4u6s-M
    * INSANE KILLSTREAKS - ROBLOX MOVIE - Stream Highlights \#1
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=gmgDzLWxZuQ
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
    * BACON HAIR FINDS THIS LEGENDARY IN POKEMON BRICK BRONZE!!
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=KTNKdtuqWsg
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
    * UNLIMITED FREE NEW ULTRA BEAST PETS IN NINJA LEGENDS!! \*Roblox\* (Giveaway)
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=Z93A9lAF90E
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

## Conclusion
The inconsistent enforcement of the Roblox terms of use and laws in the United States needs to be addressed to improve
community safety and improve the relations of the influencer program and developers. Robux giveaways and phishing can
give legitimacy to malicious people trying to get access to users' accounts while information collection puts the
information of minors at risk. Addressing these will make developers have more confidence in moderation and Roblox's
marketing team.