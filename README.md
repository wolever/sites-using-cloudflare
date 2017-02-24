# List of Sites possibly affected by Cloudflare's #Cloudbleed HTTPS Traffic Leak

This is a (work-in-progress) list of domains possibly affected by the [CloudBleed HTTPS traffic leak](https://blog.cloudflare.com/incident-report-on-memory-leak-caused-by-cloudflare-parser-bug/).
Original vuln [thread](https://bugs.chromium.org/p/project-zero/issues/detail?id=1139) by Google Project Zero.

### DISCLAIMER:
This list contains *all* domains that use cloudflare DNS, not just the cloudflare proxy (the affected service that leaked data).  It's a broad sweeping list that includes everything.  Just because a domain is on the list does not mean the site is compromised, and sites may be compromised that do not appear on this list.

Cloudflare has not provided an official list of affected domains, and likely will not due to privacy concerns.  I'm compiling an unofficial list here so you know what passwords to change.

## Impact

**Between 2016-09-22 - 2017-02-18 passwords, private messages, API keys, and other sensitive data were leaked by Cloudflare to random requesters.**
Data was cached by search engines, and may have been collected by random adversaries over the past few months.

Requests to sites with the HTML rewrite features enabled triggered a pointer math bug. Once the bug was trigerred the response would include data from ANY other cloudfare proxy customer that happened to be in memory at the time. Meaning a request for a page with one of those features could include data from Uber or one of the many other customers that didn't use those features. So the potential impact is every single one of the sites using CloudFare's proxy services (including HTTP & HTTPS proxy).

 "The greatest period of impact was from February 13 and February 18 with around 1 in every 3,300,000 HTTP requests through Cloudflare potentially resulting in memory leakage (that’s about 0.00003% of requests), potential of 100k-200k paged with private data leaked every day" -- [source](https://news.ycombinator.com/item?id=13719518)

You can see some of the leaked data yourself in search engine caches: https://duckduckgo.com/?q=+%7B%22scheme%22%3A%22http%22%7D+CF-Host-Origin-IP&t=h_&ia=web

Confirmed affected domains found in the wild: http://doma.io/2017/02/24/list-of-affected-cloudbleed-domains.html

## What should I do?

Check your password managers and **change all your passwords**, especially those on these affected sites.
Rotate API keys & secrets, and confirm you have 2-FA set up for important accounts.  This might sound like fear-mongering, but the scope of this leak is truly massive, and due to the fact that *all* cloudflare proxy customers were vulnerable to having data leaked, it's better to be safe than sorry.

Theoretically sites not in this list can also be affected (because an affected site could have made an API request to a non-affected one), *you should probably change all your important passwords*.

**Submit PR's to add domains that you know are using cloudflare, or remove domains that are not affected.**

## Methodology

This list was compiled from 3 large dumps of all cloudflare customers provided by crimeflare.com/cfs.html, and several manually copy-pasted lists from stackshare.io and wappalyzer.com.
Crimeshare collected their lists by doing NS DNS lookups on a large number of domains, and checking SSL certificate ownership.

I scraped the Alexa top 10,000 by using a simple loop over the list:

```fish
for domain in (cat ~/Desktop/alexa_10000.csv)
    if dig $domain NS | grep cloudflare
        echo $domain >> affected.txt
    end
end
```
The alexa scrape, and the crimeflare dumps were then combined in a single text file, and passed through `uniq | sort`.  I've since accepted several PRs and issues to remove sites that were unaffected from the list.

Data sources:
 - https://stackshare.io/cloudflare
 - https://wappalyzer.com/applications/cloudflare
 - DNS scraper I'm running on Alexa top 10,000 sites (grepping for cloudflare in results)
 - https://www.cloudflare.com/ips/  (going to find sites that resolve to these IPs next)
 - http://www.crimeflare.com/cfs.html (scrape of all cloudflare customers)
 - http://www.doesitusecloudflare.com/

I'd rather be safe than sorry so I've included any domain here that remotely touches cloudflare.
If I've made a mistake and you believe your site is not affected, submit a PR and I will merge it ASAP, I don't want to hurt anyone's reputation unecessarily.

You can also ping me on twitter [@theSquashSH](https://twitter.com/thesquashsh) and I'll respond as soon as I can.

## Full List

**Download the [full list.zip](https://github.com/pirate/sites-using-cloudflare/archive/master.zip) (22mb)**

4,287,625 possibly affected domains.  Download this file, unzip it, then run `grep -x domaintocheck.com sorted_unique_cf.txt` to see if a domain is present.

Also, a list of some [iOS apps](https://www.nowsecure.com/blog/2017/02/23/cloudflare-cloudbleed-bugs-impact-mobile-apps) that *may* have been affected.

## Notable Sites

- authy.com
- coinbase.com
- betterment.com
- transferwise.com
- prosper.com
- digitalocean.com
- patreon.com
- bitpay.com
- news.ycombinator.com
- producthunt.com
- medium.com
- 4chan.org
- yelp.com
- okcupid.com
- zendesk.com
- uber.com
- namecheap.com ([not affected](https://status.namecheap.com/archives/30660))
- poloniex.com
- localbitcoins.com
- kraken.com
- 23andme.com
- curse.com (and some other Curse sites like minecraftforum.net)
- counsyl.com
- tfl.gov.uk
- stackoverflow.com (confirmed not affected by StackOverflow's @alienth)
- fastmail.com ([not affected](https://twitter.com/FastMail/status/834939787924557824), [#2](https://news.ycombinator.com/item?id=13720050))
- 1password.com ([not affected](https://discussions.agilebits.com/discussion/comment/356869/#Comment_356869))

## Alexa Top 10,000 affected sites, sorted by Alexa ranking:

- upwork.com
- codepen.io
- fiverr.com
- thepiratebay.org
- extratorrent.com
- getbootstrap.com
- jquery.com
- laravel.com
- laracasts.com
- seriouseats.com
- bitdefender.com
- ziprecruiter.com
- glassdoor.com
- pastebin.com
- fitbit.com
- discordapp.com
- change.org
- feedly.com
- zoho.com
- irccloud.com

- adf.ly
- avito.ru
- theladbible.com
- popcash.net
- youm7.com
- spotscenered.info
- taringa.net
- onedio.com
- youtube-mp3.org
- digikala.com
- extratorrent.cc
- feedly.com
- subscene.com
- fiverr.com
- naij.com
- 4dsply.com
- medium.com
- hdfcbank.com
- spankbang.com
- nur.kz
- beeg.com
- 4chan.org
- udemy.com
- 2ch.net
- hespress.com
- blogfa.com
- ck101.com
- videoyoum7.com
- gyazo.com
- lapatilla.com
- wattpad.com
- sabq.org
- inquirer.net
- alwafd.org
- dostor.org
- glassdoor.com
- gamer.com.tw
- eyny.com
- drudgereport.com
- 4pda.ru
- discuss.com.hk
- almasryalyoum.com
- elfagr.org
- clixsense.com
- ptt.cc
- vetogate.com
- humblebundle.com
- animeflv.net
- fishki.net
- mangafox.me
- uptobox.com
- socialblade.com
- codepen.io
- dawn.com
- nyaa.se
- kinozal.tv
- internethaber.com
- hamariweb.com
- vanguardngr.com
- elwatannews.com
- crunchyroll.com
- nairaland.com
- cloudflare.com
- avito.ma
- efukt.com
- movieweb.com
- express.pk
- kinox.to
- ilbe.com
- anitube.se
- nmisr.com
- getbootstrap.com
- bab.la
- thisav.com
- aksam.com.tr
- pastebin.com
- cda.pl
- digitalocean.com
- imagetwist.com
- korabia.com
- hibapress.com
- opensubtitles.org
- thedailybeast.com
- shareasale.com
- ero-advertising.com
- nexusmods.com
- todayhumor.co.kr
- jquery.com
- elbotola.com
- mobafire.com
- mangastream.com
- topix.com
- standardmedia.co.ke
- ghanaweb.com
- slimspots.com
- brainyquote.com
- christian-dogma.com
- uludagsozluk.com
- primewire.ag
- rus.ec
- movie4k.to
- sunmaker.com
- heavy-r.com
- bc.vc
- sopitas.com
- allkpop.com
- tutsplus.com
- crunchbase.com
- encuentra24.com
- mangareader.net
- wikiwiki.jp
- sedty.com
- ranker.com
- thingiverse.com
- gamme.com.tw
- elitetorrent.net
- tribune.com.pk
- punchng.com
- khmerload.com
- filelist.ro
- redbubble.com
- blockchain.info
- torrentdownloads.me
- curse.com
- porntube.com
- 4tube.com
- mysmartprice.com
- moddb.com
- templatemonster.com
- zoomit.ir
- pcadvisor.co.uk
- onlinekhabar.com
- openclassrooms.com
- niusnews.com
- 2ch-c.net
- serviporno.com
- futhead.com
- express.com.pk
- minecraftforum.net
- typepad.com
- sankakucomplex.com
- statcounter.com
- forocoches.com
- almaany.com
- atwiki.jp
- proprofs.com
- funnyjunk.com
- hihi2.com
- imgchili.net
- tukif.com
- androidcentral.com
- omegle.com
- index.hr
- chomikuj.pl
- q.gs
- alphacoders.com
- tecmundo.com.br
- shiftdelete.net
- teamliquid.net
- myvidster.com
- layalina.com
- androidauthority.com
- maplestage.com
- pixhost.org
- intercambiosvirtuales.org
- puu.sh
- mathsisfun.com
- light-dark.net
- on.cc
- wpengine.com
- dailycaller.com
- twentytwowords.com
- hotukdeals.com
- uploadboy.com
- torrentfreak.com
- rawstory.com
- ixl.com
- fssnet.co.in
- soompi.com
- ziprecruiter.com
- filgoal.com
- traidnt.net
- planetminecraft.com
- jang.com.pk
- aljaras.com
- coinbase.com
- j.gs
- trafficfactory.biz
- armorgames.com
- diary.ru
- newgrounds.com
- fragrantica.com
- 2ch.hk
- mixcloud.com
- skyscrapercity.com
- proboards.com
- hugedomains.com
- seneweb.com
- dx.com
- jusbrasil.com.br
- mmo-champion.com
- jvzoo.com
- pornerbros.com
- clickbank.com
- vecteezy.com
- nguoiduatin.vn
- kwejk.pl
- noticiaaldia.com
- dardarkom.com
- adfoc.us
- laravel.com
- webs.com
- tinhte.vn
- amarujala.com
- jutarnji.hr
- menshealth.com
- vcommission.com
- mydealz.de
- smallseotools.com
- joemonster.org
- doisongphapluat.com
- tgju.org
- ojooo.com
- alternativeto.net
- uwants.com
- pingdom.com
- yepi.com
- careers360.com
- dumpert.nl
- interpals.net
- mangapanda.com
- gsmhosting.com
- tureng.com
- pccomponentes.com
- barstoolsports.com
- archive.is
- demotywatory.pl
- fontspace.com
- hubpages.com
- washingtontimes.com
- gamestorrents.com
- compucalitv.com
- zemtv.com
- anandabazar.com
- r10.net
- teespring.com
- blackhatworld.com
- iptorrents.com
- minijuegos.com
- 24sata.hr
- alison.com
- tfl.gov.uk
- experts-exchange.com
- n4hr.com
- avaaz.org
- zwaar.net
- ashleymadison.com
- addtoany.com
- seemorgh.com
- th3professional.com
- car.gr
- tamindir.com
- khabarfarsi.com
- indianpornvideos.com
- all.biz
- torlock.com
- goldesel.to
- shoutmeloud.com
- anipo.jp
- kora-online.tv
- taxheaven.gr
- hizliresim.com
- pdfonline.com
- runetki.com
- tvboxnow.com
- plugrush.com
- trueactivist.com
- nodejs.org
- hvg.hu
- 5giay.vn
- enjoydressup.com
- expatriates.com
- animeid.tv
- q8yat.com
- tn.com.ar
- 1001freefonts.com
- jotform.com
- pornbb.org
- nationalreview.com
- thenationonlineng.net
- addmefast.com
- adafruit.com
- alwatanvoice.com
- lolnexus.com
- deadline.com
- alfavita.gr
- torrentday.com
- elespectador.com
- sheknows.com
- ikman.lk
- 000webhost.com
- persiantools.com
- gezginler.net
- mediapart.fr
- thetoptens.com
- tineye.com
- advfn.com
- scamadviser.com
- waveapps.com
- gossiplankanews.com
- surveygizmo.com
- animenewsnetwork.com
- online-stopwatch.com
- creativecommons.org
- mazika2day.com
- diario.mx
- 3bmeteo.com
- gametracker.com
- share-online.biz
- almesryoon.com
- qatarliving.com
- sprashivai.ru
- zoominfo.com
- whatculture.com
- e-monsite.com
- geo.tv
- alrakoba.net
- yourbittorrent.com
- stocktwits.com
- e-estekhdam.com
- bleepingcomputer.com
- apne.tv
- aftabir.com
- nullrefer.com
- tickld.com
- dawanda.com
- musavat.com
- iconarchive.com
- seriouseats.com
- nuevoloquo.com
- blankrefer.com
- matchesfashion.com
- imore.com
- whirlpool.net.au
- mygully.com
- onlinesoccermanager.com
- mkyong.com
- frandroid.com
- dota2lounge.com
- alternet.org
- identi.li
- petapixel.com
- largeporntube.com
- destructoid.com
- bukkit.org
- fanpop.com
- notebookcheck.net
- dpstream.net
- instantcheckmate.com
- poringa.net
- haqqin.az
- hkgolden.com
- neswangy.net
- vecernji.hr
- h2porn.com
- atlas.sk
- 123telugu.com
- townhall.com
- movie-blog.org
- tomshw.it
- waseet.net
- netbarg.com
- game-debate.com
- warriorplus.com
- womenshealthmag.com
- elkhabar.com
- dnevnik.hr
- ahlynews.com
- mindmeister.com
- socialmediaexaminer.com
- rockpapershotgun.com
- smartprix.com
- filecloud.io
- fansshare.com
- prevention.com
- theregister.co.uk
- fresherslive.com
- yam.com
- realitatea.net
- rosbalt.ru
- ioffer.com
- explosm.net
- re-direcciona.me
- mitbbs.com
- behindwoods.com
- residentadvisor.net
- androidpolice.com
- linkbucks.com
- webgains.com
- uppit.com
- bitsnoop.com
- foundationapi.com
- skladchik.com
- fatakat.com
- ssense.com
- egaliteetreconciliation.fr
- informe21.com
- theme-fusion.com
- soccersuck.com
- malaysiakini.com
- hobbyking.com
- zakon.kz
- rozee.pk
- bigrock.in
- mobilism.org
- proceso.com.mx
- adult-empire.com
- listverse.com
- hardsextube.com
- filesfetcher.com
- monova.org
- n4g.com
- fuskator.com
- chatrandom.com
- jqueryui.com
- theync.com
- catracalivre.com.br
- questionablecontent.net
- xxxbunker.com
- ninisite.com
- psychcentral.com
- india-forums.com
- antena3.ro
- downloadatoz.com
- webhostingtalk.com
- bikroy.com
- qafqazinfo.az
- klix.ba
- tunisia-sat.com
- moodle.org
- charter97.org
- add-anime.net
- aristeguinoticias.com
- someecards.com
- techinasia.com
- slaati.com
- abidjan.net
- yuku.com
- dashnet.org
- mybroadband.co.za
- zetaboards.com
- slate.fr
- random.org
- clickbank.net
- dev-point.com
- novafile.com
- gameblog.fr
- hammihan.com
- brusheezy.com
- thestudentroom.co.uk
- pcgameshardware.de
- rahnama.com
- pagina12.com.ar
- digitalpoint.com
- iol.co.za
- pimpandhost.com
- radiojavan.com
- news.am
- yoo7.com
- codeschool.com
- clubedohardware.com.br
- 800notes.com
- worldtimebuddy.com
- bestblackhatforum.com
- bakufu.jp
- frmtr.com
- seslisozluk.net
- usingenglish.com
- hitleap.com
- pjmedia.com
- slashfilm.com
- whatismyip.com
- prntscr.com
- gottabemobile.com
- serienjunkies.org
- runnersworld.com
- opposingviews.com
- ap7am.com
- aporrea.org
- sm3na.com
- vavel.com
- howtoforge.com
- shorouknews.com
- bufferapp.com
- icefilms.info
- mindtools.com
- forobeta.com
- wmmail.ru
- sportdog.gr
- opencart.com
- goldenline.pl
- boards.ie
- forumophilia.com
- informationng.com
- ebs.in
- storenvy.com
- forosdelweb.com
- marunadanmalayali.com
- fakenamegenerator.com
- mediatakeout.com
- banglanews24.com
- vozforums.com
- w3resource.com
- trafficbroker.com
- hotair.com
- gistmania.com
- good.is
- hawamer.com
- memecenter.com
- jeanmarcmorandini.com
- forumactif.com
- ahlamontada.com
- edublogs.org
- sdpnoticias.com
- saaid.net
- nicozon.net
- idlebrain.com
- notdoppler.com
- opinionlab.com
- hackforums.net
- dlink.com
- insight.ly
- trojmiasto.pl
- siasat.pk
- webdesignerdepot.com
- doityourself.com
- elbilad.net
- comicbookmovie.com
- englishforums.com
- tw116.com
- foroactivo.com
- ahnegao.com.br
- forumactif.org
- zurb.com
- torrentleech.org
- warez-bb.org
- 2ip.ru
- share-links.biz
- dicelacancion.com
- diffen.com
- designboom.com
- propakistani.pk
- orgasmatrix.com
- ciudad.com.ar
- penny-arcade.com
- deutsche-wirtschafts-nachrichten.de
- shahvani.com
- latribune.fr
- lenskart.com
- agilebits.com
- wearehairy.com
- thaqafnafsak.com
- bbspink.com
- mp3xd.com
- ittefaq.com.bd
- cheathappens.com
- digital-photography-school.com
- briian.com
- annunci69.it
- resellerclub.com
- like4like.org
- sinembargo.mx
- forbes.ru
- makezine.com
- iphones.ru
- cancan.ro
- 1news.az
- postplanner.com
- freshdesignweb.com
- wayn.com
- elheddaf.com
- davidwalsh.name
- elitepvpers.com
- mforos.com
- forumotion.com
- thepoke.co.uk
- fux.com
- kn3.net
- hir.ma
- billiger.de
- sadistic.pl
- somethingawful.com
- moveon.org
- burbuja.info
- btc-e.com
- kleiderkreisel.de
- x-art.com
- plus28.com
- micromaxinfo.com
- rubias19.com
- emoneyspace.com
- marathonbet.com
- wiziq.com
- say7.info
- malwaretips.com
- al-akhbar.com
- authorstream.com
- topdocumentaryfilms.com
- sudaneseonline.com
- songlyrics.com
- kanui.com.br
- thehackernews.com
- video.az
- 10minutemail.com
- bezaat.com
- smosh.com
- infibeam.com
- 24horas.cl
- brainpickings.org
- korben.info
- puls24.mk
- avaz.ba
- elpais.com.uy
- thejournal.ie
- hostgator.com.br
- geenstijl.nl
- myorderbox.com
- fansided.com
- indiansexstories.net
- saharareporters.com
- smartinsights.com
- definebabe.com
- hiphopdx.com
- humoron.com
- nrc.nl
- infolinks.com
- javascript.ru
- forum.hr
- pcgames.de
- sportcategory.com
- smallbiztrends.com
- desidime.com
- addictinginfo.org
- naosalvo.com.br
- hostgator.in
- aitnews.com
- telelistas.net
- playxn.com
- ce4arab.com
- thenews.com.pk
- goldprice.org
- managewp.com
- erepublik.com
- freeonlinegames.com
- dreamincode.net
- brasil247.com
- ethnos.gr
- goodsearch.com
- twitchy.com
- crictime.com
- weloveshopping.com
- gtaforums.com
- jne.co.id
- holiday-weather.com
- globovision.com
- el-wlid.com
- antyweb.pl
- wehkamp.nl
- stereogum.com
- binsearch.info
- zero10.net
- 1hhhh.net
- dangerousminds.net
- simplyrecipes.com
- tech-wd.com
- mymodernmet.com
- soccermanager.com
- mixedmartialarts.com
- spi0n.com
- hockeysfuture.com
- socialmediatoday.com
- blogs.com
- 1jux.net
- maxicep.com
- prefiles.com
- wplocker.com
- ipiccy.com
- serials.ws
- xat.com
- publika.az
- grasscity.com
- lolinez.com
- viralnova.com
- alhilal.com
- colourlovers.com
- newtvworld.com
- appadvice.com
- promiflash.de
- worldtimeserver.com
- pubdirecte.com
- merca20.com
- healthkart.com
- ripoffreport.com
- broadwayworld.com
- premiere.fr
- clasicooo.com
- elephantjournal.com
- lankacnews.com
- tutorialzine.com
- sayidaty.net
- sipse.com
- thisoldhouse.com
- el-ahly.com
- impiego24.it
- bitshare.com
- gamevicio.com
- getfireshot.com
- spin.com
- allmyvideos.net
- whoishostingthis.com
- makeupandbeauty.com
- runetki.tv
- freakshare.com
- looti.net
- themelock.com
- bronto.com
- starsue.net
- thebump.com
- econsultancy.com
- cbox.ws
- jagobd.com
- dhakatimes24.com
- webhostbox.net
- laughingsquid.com
- utusan.com.my
- classifiedads.com
- freepornvs.com
- fok.nl
- freekaamaal.com
- cyanogenmod.org
- col3negoriginal.lk
- elshaab.org
- droid-life.com
- hardwareluxx.de
- graaam.com
- pijamasurf.com
- 101greatgoals.com
- appbrain.com
- indiafreestuff.in
- snapwidget.com
- putlocker.com
- ncrypt.in
- skidrowgames.net
- skidrowcrack.com
- boxden.com
- crosswalk.com
- oodle.com
- joomshaper.com
- microworkers.com
- cpalead.com
- template-help.com
- ezilon.com
- deperu.com
- designtaxi.com
- marketglory.com
- lewrockwell.com
- mg.co.za
- 1stwebdesigner.com
- worthofweb.com
- vladtv.com
- whocallsme.com
- crackberry.com
- voetbalzone.nl
- roro44.com
- thesuperficial.com
- sa.ae
- noticierodigital.com
- sexytube.me
- etxt.ru
- shoghlanty.com
- goldporntube.com
- azyya.com
- free-tv-video-online.me
- angloinfo.com
- e-cigarette-forum.com
- komikid.com
- network-tools.com
- namepros.com
- members.webs.com
- forexpeacearmy.com
- gulli.com
- joomlart.com
- mynewsdesk.com
- stadt-bremerhaven.de
- tmart.com
- lebuteur.com
- top-channel.tv
- vid2c.com
- levelup.com
- emailmeform.com
- tipsandtricks-hq.com
- davidicke.com
- celebuzz.com
- duedil.com
- chinavasion.com
- karnaval.com
- cucirca.eu
- tradetracker.com
- searchquotes.com
- xenforo.com
- it-ebooks.info
- naijapals.com
- greenwichmeantime.com
- desitorrents.com
- ktonanovenkogo.ru
- linkcrypt.ws
- liilas.com
- fakku.net
- alnaharegypt.com
- boxingscene.com
- apherald.com
- yola.com
- zenhabits.net
- seriesyonkis.com
- linkcollider.com
- gfy.com
- phpbb.com
- desmotivaciones.es
- godvine.com
- kingworldnews.com
- yeppudaa.com
- morguefile.com
- reactiongifs.com
- offervault.com
- hawkhost.com
- yougetsignal.com
- gigaom.com
- priyo.com
- jonloomer.com
- mstaml.com
- ffffound.com
- grindtv.com
- foro20.com
- utrace.de
- yucatan.com.mx
- techdirt.com
- torrentbutler.eu
- solidtrustpay.com
- cssdeck.com
- moneymakergroup.com
- gsmspain.com
- mafiashare.net
- optimizepress.com
- jquerymobile.com
- paperblog.com
- dreamamateurs.com
- 1sale.com
- b1.org
- gamefront.com
- mydigitallife.info
- realfarmacy.com
- thethao247.vn
- smartpassiveincome.com
- makeameme.org
- alfajertv.com
- themobileindian.com
- thefrisky.com
- portalnet.cl
- fap.to
- peerfly.com
- webdesignledger.com
- elance.com
- de10.com.mx
- ghost.org
- demandforce.com
- aflamneek.com
- whatsmyserp.com
- m5zn.com
- alistapart.com
- businessforhome.org
- inbound.org
- aleqt.com
- adxpansion.com
- wjunction.com
- inlinkz.com
- alltop.com
- indowebster.com
- rassd.com
- attracta.com
- freewebs.com
- nulled.cc
- linkconnector.com
- watchfreemovies.ch
- torrenthound.com
- keep2share.cc
- alweeam.com.sa
- xxxkinky.com
- trafficg.com
- forgifs.com
- addicted2success.com
- appstorm.net
- aflam4you.tv
- stuffgate.com
- zamalekfans.com
- sia.az
- robtex.com
- sethgodin.typepad.com
- pornup.me
- stansberryresearch.com
- trafficestimate.com
- imgserve.net
- matthewwoodward.co.uk
- dreamtemplate.com
- mybb.com
- reduxmediia.com
- tielabs.com
- mysavings.com
- webconfs.com
- avn.info.ve
- zone-telechargement.com
- swalif.net
- raventools.com
- game321.com
- bitcoincharts.com
- inkedmag.com
- gmane.org
- forex4you.org
- linksmanagement.com
- webmastersitesi.com
- weknowmemes.com
- cs-cart.com
- globalewallet.com
- semprot.com
- belboon.com
- ocioso.com.br
- fantasy8.com
- theme-junkie.com
- vr-zone.com
- scriptmafia.org
- source-wave.com
- hayah.cc
- mylikes.com
- divxstage.eu
- free-press-release.com
- livememe.com
- sooperarticles.com
- joomla.fr
- arouraios.gr
- pornper.com
- amino.dk
- backlinks.com
- playvid.com
- doba.com
- hotfrog.com
- daisycon.com
- paxum.com
- socialtriggers.com
- bizsugar.com
- thenewstribe.com
- watch32.com
- aktifhaber.com
- truthaboutabs.com
- whoismind.com
- kaban.tv
- hotarabchat.com
- hottube.me
- intereconomia.com
- somuch.com
- dryicons.com
- imageporter.com
- dreamteammoney.com
- systweak.com
- camplace.com
- fabthemes.com
- toprankblog.com
- avazutracking.net
- imgtiger.com
- brandyourself.com
- picstopin.com
- crocko.com
- socialadr.com
- vidspot.net
- johnchow.com
- allanalpass.com
- diariodemorelos.com
- promptfile.com
- maultalk.com
- aznews.az
- gofuckbiz.com
- searchengines.ru
- billionuploads.com
- cyberchimps.com
- optionbit.com
- peliculasyonkis.com
- problogger.net
- premiumwp.com
- myegy.com
- played.to
- sitedeals.nl
- ashleyrnadison.com
- dl-protect.com
- w4.com
- statscrop.com
- stopforumspam.com
- ann.az
- designfloat.com
- blackhatteam.com
- pbagora.com.br
- hostingflame.org
- imgdino.com
- talkarcades.com
- readms.com
- subtitulos.es
- vodly.to
- odesk.com
- doostiha.ir
- rapradar.com
- prlog.ru
- diariocontraste.com
- siyahgazete.com
- webresourcesdepot.com
- submissionwebdirectory.com
- the-bux.net
- mediatraffic.com
- cleanfiles.net
- blekko.com
- cricfree.tv
- sitetalk.com
- yazete.com
- torrentreactor.net
- natunbarta.com
- life.com.tw
- stepashka.com
- hugefiles.net
- rapgenius.com
- imagecurl.org
- blogcatalog.com
- blinklist.com
- likesasap.com
- stagram.com
- copacet.com
- indiangilma.com
- enwdgts.com
- islammemo.cc
- phimvang.com
- videarn.com
- extratorrent.com
- zemanta.com
- egyup.com
- bancdebinary.com
- arabsh.com
- racing-games.com
- likes.com
- z6.com
- games.la
- sergey-mavrodi.com
- divxplanet.com
- fun698.com
- boo-box.com
- memedad.com
- krucil.net
- dsdomination.com
- wphub.com
- wed168.com.tw
- wiziwig.tv
- blip.tv
- hightrafficacademy.com
- ofreegames.com
- 96down.com
- getcashforsurveys.com
- imgchili.com
- clip.vn
- djmaza.info
- abs-cbnnews.com
- xbmc.org
- lik.cl
- penguinvids.com
- minutebuzz.com
- tvrage.com
- articlesnatch.com
- nowgamez.com
- torrents.net
- super.ae
- fsiblog.com
- gaaks.com
- fotka.pl
- epidemz.net
- mp3skull.com
- bubblews.com
- legiaodosherois.com.br
- softarchive.net
- gigporno.com
- haivl.com
- rghost.ru
- videomega.tv
- desirulez.net
- tubeplus.me
- feedio.net
- series.ly
- antarvasna.com
- libertagia.com
- justunfollow.com
- bdr130.net
- samanyoluhaber.com
- funnymama.com
- pik.ba
- getglue.com
- fotolog.net
- anime44.com
- osdir.com
- fenopy.se
- stafaband.info
- eatlocalgrown.com
- fanswong.com
- downloads.nl
- burnews.com
- desi-tashan.com
- stadelahly.com
- watchcartoononline.com
- zaman.com.tr
- yeslibertin.com
- gfxtra.com
- gogoanime.com
- vodonet.net
- socialmediabar.com
- kinogo.net
- kalahari.com
- inews.gr
- statmyweb.com
- freelanceswitch.com
- torrentcrazy.com
- any.gs
- te3p.com
- sourtimes.org
- cima4u.com
- theelevationgroup.com
- listcovery.com
- peb.pl
- purpleporno.com
- katproxy.com
- tv-series.me
- privatehomeclips.com
- themalaysianinsider.com
- vivas.fi
- filenuke.com
- el-balad.com
- ads-id.com
- alexaboostup.com
- amadershomoybd.com
- argentinawarez.com
- arioo.com
- azertag.com
- babyoye.com
- bicaps.com
- binaryoptionsnewbies.com
- brooonzyah.net
- business2blogger.com
- buzztheme.net
- cmse.ru
- cpasbien.com
- cpasbien.me
- cyberpresse.ca
- defaultsear.ch
- defencenet.gr
- dizi-mag.com
- downloadming.me
- drakulastream.eu
- dramasonline.com
- eldia.com.ar
- excellentbux.net
- extabit.com
- eztv.it
- filmey.com
- filmifullizle.com
- freepatriot.org
- frombar.com
- fuckbooknet.net
- fullhdfilmizle.org
- genteflow.com
- gezinti.com
- gooddrama.net
- gun.az
- haivl.tv
- iitv.info
- ijreview.com
- ilyke.net
- index-of-mp3s.com
- iphoneogram.com
- italiafilm.tv
- klicktel.de
- kure.tv
- limetorrents.com
- lumfile.com
- mangahere.com
- manoto1.com
- megashare.info
- meme-lol.com
- pandodaily.com
- patient.co.uk
- pcinpact.com
- peliculas4.com
- pirateproxy.net
- pirateproxy.se
- piratestreaming.tv
- porntubevidz.com
- putlocker.bz
- putlocker.ws
- q-ask.com
- qaynar.info
- relink.us
- ryushare.com
- searchere.info
- seenive.com
- seozenlaunch.com
- sergey-mavrodi-mmm.net
- sergeymavrodi.com
- shortp.com
- siliconrus.com
- songspk.cc
- songspk.name
- stargazete.com
- stream-tv.me
- streamhunter.eu
- t411.me
- tnr.com
- tracklab101.com
- trndsys.co
- tuvaro.com
- unitezz.com
- videopremium.tv
- watchseries-online.eu
- watchseries.lt
- web-opinions.com
- wpcentral.com
- wpmu.org
- yougetpaidfast.com
- youtradefx.com
- zalukaj.tv
