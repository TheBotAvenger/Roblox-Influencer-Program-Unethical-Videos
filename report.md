# Roblox Influencer Program Unethical Videos
Generated December 15, 2024<br>
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
* Total videos: 949,344 videos
* Total videos found that match keywords: 56,229 videos
  * Total unprocessed videos: 10,258 videos
* Total videos found that are processed and marked: 457 videos 
  * Information Collection: 414 videos
  * Non-Giftcard Robux Giveaways: 34 videos
  * Other: 7 videos
  * Phishing: 2 videos

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
* BloxHub (PandaLemonTart)
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
* 🔥 OurFire Plays (ourfire and ChrisOurFire)

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
* JeffBlox (JeeffBlox)
  * Information Collection
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
    * ROBLOX  -  HELICÓPTERO VS BUGATTI CORRIDA NO JAILBREAK
      * Description references a video about the data collection website bloxawards.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=UJ9mV3bo634
* Lyna (Lynitaa)
  * Non-Giftcard Robux Giveaways
    * REGALO 100.000 ROBUX SI PIERDO ESTE RETO EN ROBLOX
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=ZNZLqUPWnfc
* Muneeb (ItsMuneeeb)
  * Phishing
    * (GONE WRONG) HACKING A FAN AND GIVING THEM FREE ROBUX!!! | Roblox
      * URL: https://www.youtube.com/watch?v=NJzfMDUMmMw
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

## Conclusion
The inconsistent enforcement of the Roblox terms of use and laws in the United States needs to be addressed to improve
community safety and improve the relations of the influencer program and developers. Robux giveaways and phishing can
give legitimacy to malicious people trying to get access to users' accounts while information collection puts the
information of minors at risk. Addressing these will make developers have more confidence in moderation and Roblox's
marketing team.