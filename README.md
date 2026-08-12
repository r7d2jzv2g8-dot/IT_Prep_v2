# IT Prep

A self-contained study system for computer networking, operating systems,
programming and security fundamentals. One HTML file, no server, no build step,
no network access required. Open it and it works.

Built for someone preparing for a broad IT knowledge exam, but useful to anyone
who wants spaced-repetition practice over this material.

---

## What is in it

| | |
|---|---|
| **71 topics** | across six content areas |
| **1,900 questions** | multiple choice, ordering, matching and free text |
| **21 readings** | with diagrams, worked examples and cited sources |
| **31 games** | drills, labs and case studies, most generating fresh boards each run |
| **12 labs** | hands-on tasks with feedback |
| **59 topic quizzes** | a 12-question check before moving on, 10 to pass |
| **6 section exams** | 30 questions each, sampled across every topic in the area |

### The six content areas

1. **Fundamentals and Theory of Computing** — number systems and binary logic,
   CPU concepts and endianness, memory organization and offsets, storage,
   kernel and user space
2. **Network Design Fundamentals** — IP and subnetting, Ethernet, routing,
   protocol layering, topology, packet transit, ports and flags, the TCP
   handshake, switches and VLANs
3. **Networking Applications and Services** — services and their ports, DNS,
   DHCP, ARP, Kerberos, encryption fundamentals, and reading the output of
   `netstat`, `ip`, `route` and `iptables`
4. **Operating Systems** — shells and shell usage, file systems and
   permissions, the Windows registry, services on both Windows and Unix,
   integrity protection, code signing, device drivers, executable formats
5. **Programming: Scripting and Interpreted** — reading code, tracing loops,
   shell scripting, interpreted languages
6. **Security** — malware classes, observable fingerprints, rootkits by
   privilege level, security products, network monitoring, vulnerabilities

---

## How it works

**Read, then practice.** Every topic offers both. Readings explain the
mechanism, show a worked example, and end with links to the primary sources —
RFCs, IEEE standards, NIST publications, vendor documentation and man pages.
Practice questions link back to the reading that covers them.

**Questions are scenario-driven.** Rather than asking what a command does, they
show its output and ask what it means:

> A host shows this:
>
> ```
> Proto Local Address        Foreign Address      State
> TCP   0.0.0.0:445          0.0.0.0:0            LISTENING
> TCP   10.0.0.5:49712       93.184.216.34:443    ESTABLISHED
> TCP   10.0.0.5:49713       10.0.0.9:3389        SYN_SENT
> ```
>
> What is the machine doing on port 3389?

**Explanations say why**, not just which. Where it matters they also say why the
tempting wrong answer is wrong, since that is the discrimination an exam
actually measures.

**Every topic follows the same four steps**, shown in order on its row: **Read**
the mechanism, **Play** a game that makes you use it, **Practice** in the exam's
own form, then **Check**. The next step is highlighted and finished ones are
ticked, so the row tells you where you are rather than offering four equivalent
doors.

Nothing is gated. Ordering is a suggestion, not a lock, because gating would
block someone revising a topic they already know. Topics without a game or a
reading show fewer steps rather than a dead button &mdash; 35 of 59 currently
have a game, and Fundamentals and Programming are complete.

**Check before you move on.** Every topic offers a quiz alongside Read and
Practice. It is deliberately not practice: twelve questions, one pass, no
running commentary, and 10 to pass.

The length is not arbitrary. With four options a guess lands one time in four,
so eight questions with six to pass let someone who genuinely knew 60% of the
material pass **55% of the time** — a coin flip dressed as a check. Twelve with
ten brings that to 25% while still passing a genuine 90% about 94% of the time. Failing it offers the reading again
rather than just a score, so "I finished the topic" and "I know the topic" stop
being the same claim.

**Spaced repetition.** Anything you get wrong is scheduled for review. Progress
is stored in the browser and can be exported and imported as JSON.

---

## Visual aids

Diagrams follow one rule: **a visual must show something the question cannot
say without giving away the answer.** A diagram that draws the answer is worse
than no diagram.

In practice that means the visual shown *before* you answer explains the
mechanism, and anything that would resolve the question waits until after:

- **Signed or Not** shows one byte marked on two number lines — unsigned and
  signed — without labeling either position with a value. The arithmetic is
  still yours; what the diagram removes is the confusion about why there are
  two readings at all. After you answer, it shows how a negative is built and
  why the encoding was chosen.
- **Longest Prefix Match** draws each route's prefix as a bar over the
  destination address, but only once you have chosen — the bar lengths *are*
  the answer.
- **Bit Bench** shows the place-value scaffold with the bits left blank.
- **Trace the Script** shows the three parts of a loop, which is equally true
  of any script and so cannot be used to shortcut one.

---

## Running it

```
open it-prep.html
```

That is the whole procedure. It is a single file with everything embedded, so
it works offline, from a USB stick, or anywhere a browser runs.

Progress lives in browser storage for that file. **Session ▸ Export progress**
writes a JSON file; **Import progress** reads one back, which is how you move
between machines or browsers.

---

## Building from source

Content lives in JavaScript pool files, readings are generated from a Python
script, and one assembler stitches everything into the single HTML output.

```
python3 cnt-readings.py      # build the readings
python3 it-assemble.py       # assemble the dashboard
```

| file | holds |
|---|---|
| `cnt-fund.js`, `cnt-os.js`, `cnt-sec.js`, ... | question pools by content area |
| `cnt-output.js`, `cnt-output2.js` | command-output interpretation questions |
| `cnt-games.js`, `cnt-visuals.js` | generated games and their diagrams |
| `cnt-readings.py` | the readings, their figures and their citations |
| `it-assemble.py` | assembles everything into `it-prep.html` |

---

## Checks

The content is large enough that eye-review is not sufficient, so the build has
audits that run against the assembled file. Each takes the finished dashboard
and exercises it the way a reader would.

| check | fails when |
|---|---|
| `poolcheck.js` | a question has fewer than four distinct options, an invalid answer index, a thin explanation, a repeated sentence, or an answer conspicuously longer than the alternatives |
| `factcheck3.py` | a stated port number, administrative distance, EtherType, protocol number, conversion or sum disagrees with a reference table |
| `readcheck.js` | a reading fails to open, renders no content, or cites fewer than three real sources |
| `topicclick.js` | any topic row is unreachable or does not offer both Read and Practice |
| `howto.js` | a game has no how-to, or its how-to does not open |
| `gamecheck.js` | a generated board has duplicate options or an answer missing from them |
| `examcheck.js` | a section exam repeats a question, misses a topic, or pulls one from another section |
| `vischeck.js` | the correct answer appears in a diagram shown before the question is answered |
| `fblink.js` | an explanation does not link to the reading that covers it |

Two of these exist because of specific mistakes. `vischeck.js` was written after
a diagram was found to encode the answer it was meant to illustrate.
`poolcheck.js` gained the option-length rule after twenty-three questions turned
out to be answerable by picking the longest choice without reading them.

---

## License

The code is provided as is. Content is written for study and cites its sources;
the linked standards and documentation remain the property of their respective
publishers.
