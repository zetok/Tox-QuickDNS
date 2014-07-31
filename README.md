## !wafuu8CaXg's Guide to Personal Tox DNS Hosting

### 1. Preface

You stare at your personal website - an impeccably designed, gargantuan
catastrophe of JavaScript, built on a grand total of seven distinct
frameworks. They exist to make *coding* easier, right? Therefore, you
should use as many as possible to get closer, closer to your
*programming zen*.

You swipe up with two fingers on your MacBook Pro™'s trackpad.
The scroller inches toward the bottom of the window excruciatingly slow -
not because there is a lot of content on the page, but because of the
hundreds of JavaScript scroll event handlers competing for processor
time.

*"Perfect."* you think to yourself.

The cell-pjotr on your desk begins to ring, playing a relaxing 40-second loop
of Boards of Canada. There is a five-dollar bill in your pocket that
you intended to give to the first person you meet who recognized the tune.
And there it sat, untouched for the past *five years*.

You answer it.

A thick Slavic accent comes through the speaker.

*"Tox is add usernam."*

*"Yes."* you respond. The line goes dead.

### 2. Preface II

This is big. **Tox usernames!** You close your eyes and try to imagine
all the ramifications of such an advancement in technology.

Not a single thing comes to your mind after ten minutes. That's okay.
You walk over to the standing desk you bought on recommendation of
someone at a hackathon, and stand at it. Wouldn't want to wrinkle your
$100 pair of jeans by sitting down, right?

A poster on the wall reminds you to check your privileges
(please check all that apply to you and submit a pull request).

- [ ] White
- [ ] Male
- [ ] Cis
- [ ] Straight
- [ ] Thin (or fit)
- [ ] Able-bodied

The list goes on, stretching below the desk, and down to the floor.

Your open your *GitHub* news feed. A new project by Tox catches your eye.

### 3. `yuu-lite`

This is a stripped-down version of the toxme.se backend, designed to
make it impossibly easy to start serving your Tox ID from your personal
domain(s).

The key advantage of using yuu-lite over static records is that it
supports tox3 encrypted lookup.

**Dependencies**:

- Python 3.x
- PyNaCl
- dnslib

Please install them from PyPI. (`pip install -r requirements.txt`)

### 4. Configuration

> *心配しないで。*  (TL note: "Don't worry")  

Configuration is hundreds of times easier than getting a mail server
to work.

#### At your DNS host

You need to add a NS record pointing to your server for
`_tox.[your domain here]`.

It would look like this in BIND format.  
`_tox	3600		IN	NS	cerberus.zodiaclabs.org.`

#### yuu-lite

Paste the following into config.json; tweak as needed.

```
{
    "pid_file": "pidfile.dl", 
    "dns_record_ttl": 60, # TTL only applies to tox1 lookups.
    "hostname": "some.domain.example.com", # Should resolve to the server you're running yuu-lite on.
    "dns_listen_addr": "",
    "dns_listen_port": 53,
    "suid": "stal:nogroup" # user:group chmod syntax
}
```

Special note: DNS runs on port 53 - you need root to bind to it.
I strongly recommend having the server setuid after binding.

#### Adding Tox IDs

Use the following format in names.json.

```
{
    "stal@zodiaclabs.org": "ABCDEF...",
    "natsuki@kirara.ca": "AABBCC..."
}
```

Finished. That didn't take long, did it? To test, compile uTox with your
tox3 key and try the names you added above.
