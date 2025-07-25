# Roblox Influencer Program Unethical Videos
Generated July 25, 2025<br>
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
* Total videos: 1,026,643 videos
* Total videos found that match keywords: 57,795 videos
  * Total unprocessed videos: 11,773 videos
* Total videos found that are processed and marked: 122 videos 
  * Non-Giftcard Robux Giveaways: 42 videos
  * Information Collection: 40 videos
  * Other: 39 videos
  * Phishing: 1 video

### No Videos Found
The following channels had nothing appear with manual searching. Videos may exist, but were not found.
* 39jeshi (39jeshi)
* 3SB Games (cakemiix and 3SBMiichael)
* Aati Plays (AatiPlays_Official)
* AbsintoJ (AbsintoJYT)
* Acenix (roblox\_user\_908886998)
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
* Cerso (Cerso93)
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
* FishyBlox (roblox\_user\_1272625300)
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
* Complex Roblox (IamComplexRoblox)
  * Non-Giftcard Robux Giveaways
    * 10k robux giveaway?!!!
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=C-LnZh8016o
    * 15k Robux Giveaway 🤩👏
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=wYWTj1BzwHY
    * 10k ROBUX GIVEAWAY!? 🤩😍✨
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=hLU4V3Q_MR0
    * FREE ROBUX GIVEAWAY..🤩🤗 \#roblox \#shorts
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=HpaPH0FsG-c
    * FREE ROBUX GIVEAWAY-?!
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=SGQHBpt-GeM
* DefildPlays (DefildPlays)
  * Information Collection
    * SECRET 24 Hour Life Hack Boost Codes In Clicking Legends! Roblox
      * Description references the data collection website p4f.gg.
      * URL: https://www.youtube.com/watch?v=VWr5ypqBhn8
    * HOW OP IS THE LEGENDARY DRILL AND SCOOPS!? \*6500$+ EACH SECOND!!\* - Roblox treasure hunt simulator
      * Description references the data collection website oprewards.com.
      * URL: https://www.youtube.com/watch?v=zE9pOAmeewE
  * Non-Giftcard Robux Giveaways
    * SECRET OWNER TURKEY PET CODES IN SABER SIMULATOR! Roblox
      * Description references a Roblox group for Robux giveaways.
      * URL: https://www.youtube.com/watch?v=l3WmiGs8MmQ
* IntelPlayz (IamRoBuilder)
  * Information Collection
    * HE MADE A DISSTRACK ON ME (Reaction)
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=ZwR0ub0qmJo
    * CREATING A GIANT PENGUIN! | Pet Simulator
      * Description references the data collection website irobux.com.
      * URL: https://www.youtube.com/watch?v=JA8rR5LLRYg
* JustHarrison (JustHarrisonYT)
  * Non-Giftcard Robux Giveaways
    * (OVER) 2,500 ROBUX GIVEAWAY!!! LAST CHANCE TO GET TOXIC GUNNER (Tower Defense Simulator - ROBLOX)
      * Uses group funds to give away Robux.
      * URL: https://www.youtube.com/watch?v=Qnq6AuArpdM
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
* XenoTy (xXenoTy)
  * Information Collection
    * Spending 10,000 Robux To Get 0.1% Limitless & Becoming Gojou Satoru In Jujutsu Piece...
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=8uivtso9wqI
    * I Survived as 0.1% Mangekyo OBITO in NEW Roblox Naruto Game! (The Time Of Ninja)
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=WxNvYELGMgk
    * Rellgames Secretly BUFFED The STRONGEST Akuma In Shindo Life! \*Broken\*
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=Yh_XBSC6RaY
    * NO WAY! Rellgames Made EVERY Limited Bloodline Spinnable & Rellcoin code REVEALED In Shindo Life
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=88WJx0DQ7wc
    * Rellgames Secretly Added The BEST Feature & You Didnt Know In Shindo life
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=SkdSV7wQJ88
    * \[CODE\] Kamaki 3rd Stage Vs Jinshiki | Which Is Better? Shindo Life
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=P3xUWgIMvqk
    * Shindo Life’s 2 Year Anniversary & 250k Rellcoin Challenge | Shindo Life Rellgames
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=YfszXV8h5L0
    * \[CODE\] Flame Element REWORK Full Showcase Shindo Life Rellgames
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=yulhSQZTbXo
    * GEN 3 BATTLE! Apol Vs Maru Vs Sparky! |  Shindo Life Rellgames
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=DPWh_rl98ps
    * SnakeMan Platinum Bloodline Full Showcase | Shindo Life Rellgames
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=RuYHZNaAozQ
    * This NEW Naruto Game On Roblox HAS EVERYTHING YOU WANT!
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=0zYzvn_SgvA
    * \[CODE\] The Shisui Uchiha Experience In This New Roblox Anime Game (AU REBORN)
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=uJhvPBDRh7c
    * This NEW Roblox Anime GAME JUST RELEASED!
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=YfMoDALRIoQ
    * \[CODE\] The Deva Rengoku Rengoku Rework Will Make One Piece Fans SALTY In Shindo Life!
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=7cipNtmdbxE
    * \[CODE\] Rellgames Just Made This The Strongest Bloodline....Or Did They? | Shindo Life
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=f9ucXoIKEaA
    * GET This God Bloodline Before It Gets REWORKED This Upcoming Update On Shindo Life!
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=YCaPk2vc6-A
    * Top 5 Strongest Bloodline's In Shindo Life! | Shindo Life Bloodline Tierlist
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=Fqxb6Hypp0I
    * Get This FREE BLOODLINE BAG Feature In This NEW Shindo Life Update!
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=fUNIixJRC9I
    * \[Shindo\] The 24 Hour Momoshiki Experience In Shindo Life | Rellgames
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=Si8whY0x1pw
    * Bruce Kenichi Vs Ryuji Kenichi! Which Is Better? Shindo Life
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=gNJbNR5m9Ck
    * I Became A GOD In 2022 NEWEST Roblox Anime Game!
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=z7y1UYxavC0
    * \*MAX\* Raion Gaiden Sengoku Bloodline FULL SHOWCASE! Shindo Life | Rellgames
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=8xQiI7FEnjc
    * \[CODE\] These 2 TOXIC & OP GOD BLOODLINES DESTROY NOOBS In 2v2 COMP! | Shindo Life |Shindo Life Codes
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=GVvcokTFLNQ
    * \[CODE\] EASTWOOD KORASHI VS GHOST KORASHI! Which Is Better? | Shindo Life | Shindo Life Codes Roblox
      * Description references the data collection website claimrbx.com.
      * URL: https://www.youtube.com/watch?v=k1JcYZO7cnc
    * \[CODE\] RYUJI PLATINUM VS RYUJI KENICHI! Which Is Better? Shindo Life | Shindo Life Codes
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=UxzpK6s_vs8
    * \[CODE\] 1250 SPIN CODES To Unlock THE ULTIMATE TENTACION MODE! | Shindo Life | Shindo Life Codes
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=g38nG0jjORg
    * \[CODE\] USE These CODES To Unlock THE ULTIMATE TENTACION MODE! | Shindo Life | Shindo Life Codes
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=Wwz4tAKNEdg
    * \[730K CODE\] ALL NEW 6 \*FREE SPINS\* SECRET CODES in SHINDO LIFE (Shindo Life Codes)ROBLOX Shindo life
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=Mk2Kcj27WDI
    * \[CODE\] SIX PATHS TS VS FULL SUSANOO AKUMA! Which is Better? | Roblox Shindo Life | Shindo Life
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=pg1iYIHrkE0
    * \[CODE\] ALL NEW 5 \*FREE SPINS\* SECRET CODES in SHINDO LIFE (Shindo Life Codes)ROBLOX Shindo life
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=CFEcoRWh-d8
    * Zoro & Luffy | ABA Ranked 2v2’s | Ft Sensei Maui | Anime Battle Arena | Roblox
      * Description references the data collection website gemsloot.com.
      * URL: https://www.youtube.com/watch?v=ETuqF6n3ce4
  * Other
    * (MUST PLAY) This NEW Roblox Anime Game Is The BEST In 2023..!
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=FsJb0cs-QtA
    * This NEW Roblox One Piece Game HAS The BEST DRAGON FRUIT..? (2023)
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=k8Rs8Vjl1og
    * NEW VEGETA MOVESET In Z BATTLEGROUNDS Is INSANE..! Z Battlegrounds Update
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=SpzdS8ItX4c
    * Spent 100 days Going From Noob To MADARA UCHIHA In Shindo Life! Rellgames
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=67cFSByL4ik
    * The Strongest Battlegrounds UPDATE Is FINALLY Here! Metal Bat ULTIMATE Release Date Confirmed Roblox
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=qAfpYqDkQpA
    * This NEW ROBLOX My Hero Academia Game Is On The Rise...
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=KzDQFA7Nsvg
    * Finally Unlocked This SECRET Form In Roblox Dragon Soul | Great Ape Showcase Dragon Soul
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=2DKgJJ9AAkQ
    * Going From NOOB to True Triple Katana in One Video! \[Blox Fruits\]
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=qUHTV0RMi1U
    * \[CODE\] Finally Unlocked This NEW 1% Form In Roblox Dragon Soul | Ekari Technique Full Showcase
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=z5trHVp1VgA
    * Project Slayers DAKI Blood Demon Art Is FIRE..? Obi Manipulation Showcase Reaction Project Slayers
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=5wxkp9WrQ1w
    * FASTEST Way To Get GREAT APE FORM In NEW Roblox Dragon Soul Update
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=pf5pQOPhlqU
    * \[CODE\] HURRY! Do This In NEW Roblox Dragon Soul UPDATE!
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=D4DrxcgO8hg
    * UPDATE 20 NEW FRUIT | MAMMOTH Fruit Full SHOWCASE On Blox Fruits (Concept)
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=ovHiupz7YpU
    * How To Unlock KAIOKEN FOR FORMS In NEW Roblox Dragon Soul Update!
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=ayKV8RzXxTo
    * How To Unlock KAIOKEN In NEW Roblox Dragon Soul Update!
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=P8eaHP0TRCE
    * UPDATE 20  | Control Awakening Full SHOWCASE On Blox Fruits (Concept)
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=mUe3i_BEp-o
    * This ROBLOX One Piece Game JUST REWORKED Gear 4th BOUNCEMAN (Must Try)
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=Dl2-udaA3Iw
    * I FINALLY Unlocked This 0.3% FORM In The BEST DBZ Game On Roblox! (Dragon Soul)
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=Em495Y-jOLs
    * You HAVE To Get ULTRA INSTINCT In This ROBLOX Anime Game!
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=gwMkKirEWYs
    * NO WAY! This Is Getting REWORKED In NEW BLOX FRUITS UPDATE 20?!
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=QSncb96BXOE
    * This NEW Roblox NARUTO Game Has The BEST SHARINGAN KG..? Shinobi Battlegrounds Reaction
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=6Iux_hsdpz0
    * This NEW Roblox Naruto Game Has The BEST RINNEGAN KG..?! Shinobi Battlegrounds Reaction
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=kKyctglaWws
    * This NEW Roblox Naruto Game Has The BEST OBITO KG..?! Shinobi Battlegrounds Reaction
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=Hwpev6TutvM
    * This NEW Roblox Naruto Game Has The BEST ITACHI MANGEKYOU KG..?! Shinobi Battlegrounds Reaction
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=8DhoxrF8r_k
    * Can We Get The NEW RELL BLOODLINE With 1000 Spins...?
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=IofYkYP5nx0
    * The NEW RELL BLOODLINE Is UNBELIEVABLE... | Shinobi Life 2
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=NxkXRkkoRs4
    * \[CODE\] Rykan Shizen Rework Full Showcase | Shindo Life | Shinobi Life 2 | Rellgames
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=v8-tpa8hYCs
    * This NEW Roblox Naruto Game Has The BEST MADARA KG..?! Shinobi Battlegrounds Reaction
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=2NP6wsOdoqQ
    * HURRY! Do This Before Project Slayers UPDATE 2 DROPS!
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=CtLIDO7snX4
    * Spending 100 Days As TENGEN UZUI In PROJECT SLAYERS...
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=IkgljG6MeTY
    * This NEW One Piece Roblox Game RELEASES SOON...!
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=cgN0SQbXV2Q
    * Shinobi life 2 Update Release Date | Rell Bloodline Pre Showcase | NEW EVENT | Rellgames Shindo life
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=PXwou60gVO0
    * (CODE) Light Fruit Full Showcase | Pixel piece
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=5LFfpYIaXdA
    * You NEED To Try This NEW Roblox Chainsaw Man BATTLEGROUNDS GAME!
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=fOlw6yVJm_A
    * The Story Of RELLGAMES Is INSANE...
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=hUmRgH3PgOU
    * Becoming AKAZA in 24 Hours (Project Slayers)
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=iXos_EIzlys
    * Spending 100 Days As MUCHIRO Tokito In PROJECT SLAYERS...
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=3DdqUzm3IfM
    * (CODE) FISHMAN KARATE Full Showcase Pixel Piece
      * Description references the Roblox gambling website rbxgold.com.
      * URL: https://www.youtube.com/watch?v=WY2nINj2uF4
    * (CODE) Daki's BDA Is CONFIRMED Coming To Project Slayers UPDATE 2!
      * Description references the "black market for limiteds and Robux" ro.place.
      * URL: https://www.youtube.com/watch?v=ti_3hA7bHcg

## Conclusion
The inconsistent enforcement of the Roblox terms of use and laws in the United States needs to be addressed to improve
community safety and improve the relations of the influencer program and developers. Robux giveaways and phishing can
give legitimacy to malicious people trying to get access to users' accounts while information collection puts the
information of minors at risk. Addressing these will make developers have more confidence in moderation and Roblox's
marketing team.