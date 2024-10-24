# Roblox Influencer Program Unethical Videos
Generated October 24, 2024<br>
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
* Total videos: 910,483 videos
* Total videos found that match keywords: 55,985 videos
  * Total unprocessed videos: 10,014 videos
* Total videos found that are processed and marked: 626 videos 
  * Information Collection: 546 videos
  * Other: 45 videos
  * Non-Giftcard Robux Giveaways: 34 videos
  * Phishing: 1 video

### No Videos Found
The following channels had nothing appear with manual searching. Videos may exist, but were not found.
* 39jeshi (39jeshi)
* 3SB Games (cakemiix and lcespicebabyyy)
* Aati Plays (AatiPlays_Official)
* Abbaok (Abbaok_YT)
* AbsintoJ (AbsintoJYT)
* Acenix (AcenixElMejorYoutube)
* AEREN (AaronDRonin)
* AidanGamerHD (AidanGamerHDXD)
* Alaska Violet (alaskaviolett)
* Alex Crafted (HeyCrafted)
* Aline Games (AliineGamesYT)
* AlphaGG (RealYouTube_AlphaGG)
* AlvinBlox (Alvin_Blox)
* Amberry (Amberrry)
* Andre Nicholas (andrhevn21)
* Angelazz (Angelazz)
* AnielicA (ANIELICA01)
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
* Cee\_berry (Cee\_berry)
* Cerso (Cerso93)
* Chekecheto (Chekelete1)
* Cheo Power (cheopower)
* CherryAhrizona (baby_ahrizona)
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
* KingOfYouTube (uKingzaum)
* kitsubee (KlTSUBEE)
* Kitt Gaming (StarCode_kitt)
* Kodak (KodakOF)
* KraoESP (KraoESP)
* KreekCraft (RealKreek)
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
* Mandinha Game (iii_Amandinha)
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
* Nanndo (nanndoyt)
* NanoProdigy (NanoProdigy)
* Natasha Panda (NatashaPandaaYT)
* Nathy Super Gamer (Nathy_SuperGamer)
* Natsuki Sel (Natsuki_Sel)
* NavyXFlame (NavyXFlame)
* Nicole Kimmi (Nicole_Kimmi)
* Nicole Maffi (NicoleMaffi and vanessamaffi)
* NightFoxx (Night_foxx)
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
* Red Weeb (RedWEBE3)
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
* Shenty Gaming (IM_Celestial)
* Shiloh (okgamerman)
* ShisuiXI (Sh1suiXI)
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
* Stevebloxian (steven111234)
* stream CA Roblox (stream\_CA and Dominus\_CA)
* Striker180x (strikerl80x)
* Stron (StronbolDev)
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
* V2 Plays (MinniiHxpe and toreject)
* Valen Latina (Valen_Latina)
* VANILLA (UseStarCode_VANILLA)
* VarietyJay (VarietyJay_Real)
* VeD\_DeV (VeD\_DeV)
* Veyar (VeyarYT)
* ViewSIM (ViewYT)
* VikingPrincessJazmin (VikingPrincessJazmin)
* Vitamine (VitamineRoblox)
* Vitória MineBlox (vitoriamineblox11, anamineblox11, and JackieMineBlox11)
* Volt (v0_1t)
* Waike (JJ_Waike)
* WhoseTrade (WhoseTrade)
* WikiaColors (WikiaColor_s)
* WILCO (EsElWilco)
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
* BereghostGames (snapple43, pixiedust423, Bereghost, and valadin1)
  * Other
    * GOT THE RONA in ROBLOX | SNEEZING SIMULATOR
      * Video promotes a game that is based on the 2019 Novel Coronavirus  (COVID-19) pandemic, which monetizes an event that has killed people and damaged global economies and people's lives.
      * URL: https://www.youtube.com/watch?v=bstE2x5chO8
* Betroner y Noangy (Betroner and Noangy)
  * Non-Giftcard Robux Giveaways
    * Robux para ti! Mejor que los eventos de Roblox!
      * Video is a Robux giveaway.
      * URL: https://www.youtube.com/watch?v=PqnyTfRPixw
    * TRUCO! Consigue DINERO y XP muy rápido en Loomian Legacy Roblox en Español
      * Contains link to a Roblox giftcard giveaway.
      * URL: https://www.youtube.com/watch?v=0m4FxrqvE-A
* EmpireBlox (EmpireBloxOficial)
  * Information Collection
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
    * FIZ HACKER NO GUEST 666 !! DESTRUÍ ELE ??(ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=5jhaKkVZpvc
    * PASSEI A SENHA DA MINHA CONTA PARA TODOS !! (TESTE HONESTIDADE  ) ROBLOX!!
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=JXJyNDrL-3w
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
    * NOVO PRESENTE COM ITEM MISTERIOSO !! SAIU SEGUNDO PRESENTE !!! (ROBLOX)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=cY6NOpYtk98
    * NOVOS OBSTÁCULOS NO BANCO DO JAILBREAK !! NÃO VOU MAIS ROUBAR NO BANCO !! (DIFÍCIL)
      * Description references the data collection website bloxawards.com.
      * URL: https://www.youtube.com/watch?v=r1d-74jigO0
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
    * JAILBREAK CAIRÁ PARA SEMPRE?? (ROBLOX) \#EMPIRENOTICIA !!
      * Description references a Roblox Cheat Engine bypass using mega.nz.
      * URL: https://www.youtube.com/watch?v=DwwMpLw8Wh4
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
* JeffBlox (JeeffBlox)
  * Information Collection
    * VAI TER MAIS CÓDIGOS DE ITENS GRÁTIS NO ROBLOX?
      * Description references the data collection website claimrbx.com using a link shortener.
      * URL: https://www.youtube.com/watch?v=o2zMR7gttrk
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

## Conclusion
The inconsistent enforcement of the Roblox terms of use and laws in the United States needs to be addressed to improve
community safety and improve the relations of the influencer program and developers. Robux giveaways and phishing can
give legitimacy to malicious people trying to get access to users' accounts while information collection puts the
information of minors at risk. Addressing these will make developers have more confidence in moderation and Roblox's
marketing team.