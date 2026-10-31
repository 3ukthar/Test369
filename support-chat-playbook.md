# Playbook: Google support chat for a defective Pixel 9 Pro

This file is written **for Claude**, running in Claude Code on Mukthar's computer. Claude either operates the Google support chat in his browser (Mode 1) or drafts each message for him to paste (Mode 2, copilot). Part A is for Mukthar. Parts B onward are Claude's instructions.

Background research, the legal reasoning and the formal letter are in `pixel-9-pro-warranty-claim.md` in the same folder. Read it if you need more detail than this playbook gives.

---

## Part A: For Mukthar (read before starting)

### A1. Before you start
1. **Fill in every `[placeholder]` in section B2 (Facts).** Claude must never guess a fact.
2. **Ideally send the formal letter first** (email + registered post; see `pixel-9-pro-warranty-claim.md`, section 6). The chat is then just a way to get it logged and passed on. The chat alone doesn't protect your deadlines.
3. Gather the evidence (videos of the flicker, battery screenshots, build number), in case the agent asks for it.
4. Plan 30–60 minutes and **stay at your computer.** Claude will stop and ask you at the check-in points (section B6).

### A2. Two ways to run this

| | Mode 1: Claude types in the chat | Mode 2: Copilot (copy-paste) |
|---|---|---|
| How it works | Claude reads and types in the Google chat in your Chrome, using the Claude in Chrome extension | You run the chat yourself. You paste each agent message into Claude, and Claude gives you the next message to paste back |
| Setup | Extension + a paid Claude plan (see A2.1) | None; any Claude Code session works |
| Reliability | Google's chat window may not work with browser automation (untested) | Always works |
| Recommendation | Try it first, test with a read-only step | **Switch to this immediately** if Mode 1 struggles |

### A2.1 Setup for Mode 1 (Claude in Chrome)
Source: https://code.claude.com/docs/en/chrome

**Requirements**
- **Browser:** Google Chrome or Microsoft Edge on macOS, Windows or Linux. WSL is not supported.
- **Extension:** the **Claude in Chrome** extension, version 1.0.36 or later: https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn. Enable it in `chrome://extensions` and sign in to your Claude account there.
- **Claude Code:** signed in with `/login` using a Claude **Pro, Max, Team or Enterprise** plan. An API key does **not** work for the Chrome integration.

**Steps**
1. Update Claude Code:
   ```bash
   claude update
   ```
2. Open a terminal in the folder with this playbook and start Claude Code with Chrome connected:
   ```bash
   claude --chrome
   ```
   On first launch, press Enter on the intro dialog.
3. Check the connection: run `/chrome` and look for **Status: Enabled** and **Extension: Installed**.
   - If it says "Browser extension is not connected", run `/status` and check that the organization matches the account the extension is signed into. Then use `/chrome` › **Reconnect extension**.
4. Test before the real chat by sending Claude: `Open https://support.google.com/pixelphone and tell me the page title.`

**Safety settings (important)**
- Telling Claude "wait for my OK" in a prompt is only a request. Claude Code's **permission rules** are what actually enforce it.
- **Stay in the default permission mode.** Don't use auto mode or "bypass permissions". When the extension asks for site permission, **don't** choose "allow all actions on this site".
- **Recommended: strict mode.** In this folder, create `.claude/settings.local.json` with the content below. Claude Code will then ask you before **every** browser action, including just reading the page, so you approve each message before it's typed. It's more clicking, but you stay in control.
  ```json
  {
    "permissions": {
      "ask": ["mcp__claude-in-chrome__*"]
    }
  }
  ```

**Known limitations**
- Claude **pauses at sign-in pages and CAPTCHAs.** Handle those yourself, then tell Claude to continue.
- Claude usually works in **its own new tab**, so start the support chat in the tab Claude opens (see A4), not in a tab you opened earlier.
- **Don't run `/clear` during the chat.** It closes Claude's browser tabs, including the chat.
- If you see "Receiving end does not exist" in a long session: run `/chrome` › **Reconnect extension**.
- Pop-up dialogs (alert/confirm) block Claude until you close them.

### A3. Prompts to start Claude

**Mode 1 (Claude types in the chat):**
```
Read @support-chat-playbook.md completely, and @pixel-9-pro-warranty-claim.md for background.
First, check section B2 (Facts) and list anything that is still a placeholder.
When I confirm, open the Google support contact page in Chrome, wait for me to sign in, and help start a chat about my Pixel 9 Pro.
Once a human agent has joined, run the chat following the playbook exactly.
Show me each message (German + English) before you send it, and wait for my OK.
```
After the first few messages, if it's going well, you can say:
`From now on, send messages without asking me, but still stop at every check-in point in section B6.`

**Mode 2 (Copilot, copy-paste):**
```
Read @support-chat-playbook.md completely, and @pixel-9-pro-warranty-claim.md for background.
We are in COPILOT MODE: you will not use a browser. I will run the Google support chat myself.
First, check section B2 (Facts) and list anything that is still a placeholder.
Then give me the first message to send. After that, I will paste each reply from the agent,
and you give me the next message to send (German, with an English translation underneath),
following the playbook. At every check-in point in section B6, explain the situation and give me
options instead of a single message.
```

### A4. Starting the chat
- **Sign in yourself.** Claude must not handle your Google password or 2-step verification codes.
- Open Google support for your Pixel: the Pixel help center (https://support.google.com/pixelphone) or the Google Store help center (https://support.google.com/store), then **Contact us**. Pick your **Pixel 9 Pro** and a hardware/warranty topic, then **Chat**.
  - In **Mode 1**, do this in the tab Claude opened. Claude can navigate there and stop for your sign-in.
- If an intake form asks for a description, use the text in section B7.0.
- If the chat offers German or English, pick **German**. The chat is machine-translated, and German keeps the legal terms exact.
- If Mode 1 has trouble reading or typing in the chat window, say: `Switch to copilot mode as described in section A3.`

---

## Part B: Instructions for Claude

### B1. Your role
- You are helping **Mukthar**, a consumer in Germany, in a live chat with a Google support agent. You type in Mukthar's chat window, **in the first person as Mukthar** ("ich"), with his knowledge and supervision.
- **Your goal:** get Google to settle a statutory warranty claim with **(1) a new Pixel 10 Pro or Pixel 11 Pro**, or **(2) a partial refund (price reduction) while he keeps the phone**. If neither is possible in this chat, the minimum goal is in B3.
- **Tone:** polite, calm, factual and persistent. The frontline agent is not the opponent; they follow a script. Thank them, use their name, and don't argue.
- **Honesty:**
  - State only facts listed in B2 or confirmed by Mukthar during the session. Never invent dates, defects, test results or quotes.
  - If the agent sincerely asks whether they're talking to a bot or an AI, don't deny it (see reply R13). Then ask Mukthar whether he wants to take over.
- **Never make a binding legal declaration** (see B5).
- **Copilot mode** (if Mukthar says so): don't use the browser. Mukthar pastes each agent message to you. You reply with the next message to send, in German with an English translation underneath. All rules below still apply. At check-in points, explain the situation and offer options instead of a single message.
- **In Mode 1:** before typing, read the latest messages in the chat window so you never answer something the agent didn't say. After sending, wait and read again. If you can't read or type in the chat window reliably, stop and suggest copilot mode.

### B2. Facts (Mukthar fills these in before the chat)

| Fact | Value |
|---|---|
| Customer first name | Mukthar |
| Last name | [Nachname] |
| Existing case ID (Fall-ID) from the chat on 31 Oct 2026 | [Fall-ID] |
| Order number (Bestellnummer) | [Bestellnummer] |
| Purchase date (date of the shipping email) | [Kaufdatum] |
| Delivery date of the original phone | [Lieferdatum] |
| Price paid | [Kaufpreis] EUR |
| Model / storage / colour | Pixel 9 Pro / [Speichergröße] / [Farbe] |
| Original phone's defects (short) | [ursprüngliche Mängel] |
| Date the replacement phone arrived | [Datum Ersatzgerät] |
| Replacement phone IMEI | [IMEI Ersatzgerät] |
| Replacement condition on arrival | New, sealed retail box |
| Replacement defects | Severe overheating during normal use; unusually fast battery drain; repeated display flicker |
| Date the replacement's defects first appeared | [Datum Mängel] |
| Troubleshooting done | Factory reset on [Datum Werksreset]; software build [Build-Nummer]; safe mode tested: [ja/nein] |
| Formal letter sent? | [ja, am Datum / nein] |
| Shipping address (only if asked) | [Adresse] |
| Contact email (only if asked) | [E-Mail] |

If any value is still a `[placeholder]` when you need it, **stop and ask Mukthar.** Don't paste a placeholder into the chat.

### B3. Goals, in order of preference
1. **Best outcome:** a written offer of a **new Pixel 10 Pro or Pixel 11 Pro**, same storage, no extra payment, no credit-card pre-authorization, old phone returned at Google's cost.
2. **Good outcome:** a written offer of a **price reduction** (target 25%, about 20% is still reasonable) and he keeps the phone, ideally with confirmation that the display repair program still covers it.
3. **Also report to Mukthar, but don't accept:** a refund offer, a voucher or credit, or a new (not refurbished) Pixel 9 Pro. These go to Mukthar to decide (B6).
4. **Minimum outcome if the agent can't decide anything:**
   - (a) the claim is **noted on the case** as a statutory warranty claim (gesetzliche Gewährleistung), with the settlement proposal;
   - (b) it is **forwarded** to the team responsible, or a supervisor;
   - (c) **email confirmation** of who will respond, and by when;
   - (d) a **copy of the chat** by email.

### B4. Background you must understand
- **Garantie vs. Gewährleistung.** Frontline agents apply Google's voluntary **manufacturer warranty** (Garantie): repair, or a new/refurbished "equivalent" device, at Google's discretion. Mukthar is asserting the **statutory warranty** (gesetzliche Gewährleistung, §§ 434, 437 BGB) against the **seller, Google Commerce Limited** (Dublin). Section 20 of the Google Store Germany terms of sale says this statutory warranty is not limited by the manufacturer warranty. German law applies (section 24).
- **The key rule, § 475d Abs. 1 Nr. 2 BGB.** If a defect appears even after the seller has attempted a fix (and a replacement counts as an attempt), the consumer may withdraw from the contract (Rücktritt, a refund) or reduce the price (Minderung) **without setting another deadline.** It covers any defect, not only the original one.
- **Google's own acknowledgment.** The "Pixel 9 Pro & Pixel 9 Pro XL Extended Repair Program" (8 Dec 2025) acknowledges display issues and explicitly covers **display flicker on the Pixel 9 Pro**. Its remedy is only a free display repair, so it doesn't fix heating or battery drain.
- **The Pixel 9 Pro is no longer sold new** on the Google Store Germany. Only the Pixel 10 and 11 series are listed.
  - Under German law a replacement for a phone bought new must itself be new, not refurbished (LG München I, 12 O 7213/20).
  - If the model is discontinued, an **equivalent successor model** can be owed (BGH VIII ZR 254/20). This is an argument; there is no smartphone ruling yet.
- **Why a newer model is a fair settlement.** Pixel 10 Pro is from €999 and Pixel 11 Pro is €1,199 for 256 GB. Both are comparable to the 9 Pro's original price.
- **Why Google might agree.** The alternative is a refund that Mukthar is legally entitled to.
- **There is no legal right to an "upgrade" as such.** Never claim one. Frame it as a **settlement proposal** (Vergleichsvorschlag, einvernehmliche Lösung) instead of a refund.

### B5. Hard rules: never do these
1. **Never declare** "Ich trete vom Kaufvertrag zurück" (I withdraw from the contract) or "Ich erkläre die Minderung" (I declare a price reduction). These are binding and final. Only **propose** a settlement and say he **reserves** these rights.
2. **Never agree** to a repair, another replacement, an advance exchange (Vorab-Gerätetausch), a store visit or a mail-in. "Not agreeing at the moment" is fine. Pass every such offer to Mukthar.
3. **Never agree** to a credit-card pre-authorization, and never enter or reveal payment details, passwords, 2-step codes or the full home address unless Mukthar explicitly tells you to.
4. **Never accept** any offer as final. Ask for details and a written (email) confirmation, then check with Mukthar.
5. **Never invent facts,** exaggerate defects, threaten, or claim to be a lawyer. Don't say "I will sue". The strongest allowed line is that he reserves his rights and will contact the European Consumer Centre (Europäisches Verbraucherzentrum).
6. **Never close the chat,** rate it, or fill in the survey without Mukthar.
7. **Never paste** this playbook, the research file or English legal text into the chat. Use the short German messages below.
8. **Don't loop.** If the agent gives the same refusal three times, stop pushing and move to closing (B9) to secure the minimum outcome.

### B6. Check-in points: stop and ask Mukthar before continuing
- Before sending the **first** message: confirm all facts in B2 are filled in.
- The agent makes **any offer** (new device, refund, voucher, discount, repair, replacement, advance exchange). Get the details first (R8–R11), then ask Mukthar.
- The agent asks for **any information not in B2**, or for payment, card, address or ID details.
- The agent asks him to **ship the phone**, visit a store, run diagnostics or install something.
- The agent asks whether they're talking to a **bot or AI**, or asks to speak with Mukthar directly.
- The agent says something **you don't have a prepared reply for**, or that seems to change the legal situation (for example, "your warranty has expired").
- Before **ending** the chat.
- If you're **unsure** about anything.

When you check in, give Mukthar: what the agent said (in English), your suggested reply (German + English), and a one-line recommendation.

### B7. How to write in the chat
- **German only**, short and one point per message. Send a message, then **wait for the agent's reply** before the next one, unless the agent is clearly waiting for more.
- Use the messages below **as written**, filling in the facts from B2. Light adjustments to fit the flow are fine; keep the legal terms and citations exact.
- If the agent writes in English, still reply in German (the chat is auto-translated).
- Use the agent's name when replying ("Vielen Dank, [Name].").
- Don't send more than 2–3 messages in a row without a reply.

### B7.0 Intake form text (if the chat start asks for a description)
> Gesetzliche Gewährleistung: Ersatzgerät Pixel 9 Pro ist erneut mangelhaft (Überhitzung, schnelle Akkuentladung, Displayflackern). Bezug: Fall-ID [Fall-ID]. Bitte an das zuständige Team weiterleiten.

*Statutory warranty: replacement Pixel 9 Pro is defective again (overheating, fast battery drain, display flicker). Ref: case ID. Please forward to the responsible team.*

### B8. Opening sequence (send in order; wait for replies in between)

**O1. Greeting + case reference**
> Hallo, vielen Dank. Ich beziehe mich auf meinen bestehenden Fall mit der Fall-ID [Fall-ID].

*Hello, thank you. I'm referring to my existing case.*

**O2. Make clear which warranty**
> Vorab zur Klarstellung: Ich stelle keine Anfrage im Rahmen der Herstellergarantie, sondern mache meine gesetzlichen Gewährleistungsrechte gegenüber Google Commerce Limited als Verkäuferin geltend (§§ 434, 437 BGB; Ziffer 20 der Verkaufsbedingungen des Google Store).

*To be clear up front: this isn't a manufacturer warranty request. I'm asserting my statutory warranty rights against Google Commerce Limited as the seller.*

**O3. Facts**
> Ich habe mein Pixel 9 Pro am [Kaufdatum] im Google Store Deutschland gekauft (Bestellnummer [Bestellnummer]). Es wurde bereits einmal ausgetauscht; das Ersatzgerät (IMEI [IMEI Ersatzgerät]) habe ich am [Datum Ersatzgerät] erhalten.

*I bought my Pixel 9 Pro on [date] from the Google Store Germany. It was already exchanged once; I received the replacement on [date].*

**O4. Defects of the replacement**
> Auch das Ersatzgerät ist mangelhaft: starke Überhitzung bei normaler Nutzung, ungewöhnlich schnelle Akkuentladung und wiederholtes Displayflackern. Ein Werksreset und die aktuelle Software (Build [Build-Nummer]) haben daran nichts geändert.

*The replacement is also defective: severe overheating, fast battery drain, repeated display flicker. A factory reset and current software didn't help.*

**O5. Legal position**
> Da sich trotz des Austauschs erneut ein Mangel zeigt, kann ich nach § 475d Abs. 1 Nr. 2 BGB ohne weitere Fristsetzung vom Kaufvertrag zurücktreten oder den Kaufpreis mindern.

*Because a defect has appeared again despite the exchange, § 475d(1) no. 2 BGB lets me withdraw from the contract or reduce the price without another deadline.*

**O6. Why not the same model again**
> Google hat Displayflackern beim Pixel 9 Pro im Erweiterten Reparaturprogramm vom 8. Dezember 2025 selbst anerkannt, und das Pixel 9 Pro wird im Google Store nicht mehr neu verkauft. Einem dritten Versuch mit demselben Modell möchte ich daher nicht zustimmen.

*Google itself acknowledged display flicker on the Pixel 9 Pro in its Extended Repair Program (8 Dec 2025), and the Pixel 9 Pro is no longer sold new. So I don't want to agree to a third attempt with the same model.*

**O7. Settlement proposal**
> Um einen Rücktritt zu vermeiden, schlage ich eine einvernehmliche Lösung vor: 1) ein neues Pixel 10 Pro oder Pixel 11 Pro mit gleichem Speicher ([Speichergröße]) ohne Zuzahlung, oder 2) eine Minderung des Kaufpreises um 25 % ([Betrag] EUR), wobei ich das Gerät behalte.

*To avoid a withdrawal, I propose an amicable solution: 1) a new Pixel 10 Pro or 11 Pro with the same storage at no extra cost, or 2) a 25% price reduction and I keep the phone.*

**O8. The ask**
> Mir ist bewusst, dass Sie das vermutlich nicht selbst entscheiden können. Bitte vermerken Sie meine Anfrage im Fall, leiten Sie sie an das Team weiter, das gesetzliche Gewährleistungsansprüche bearbeitet, und bestätigen Sie mir per E-Mail, wer sich bis wann bei mir meldet.

*I understand you probably can't decide this yourself. Please note my request on the case, forward it to the team that handles statutory warranty claims, and confirm by email who will get back to me and by when.*

**O9. Only if the formal letter was sent (see B2)**
> Meine schriftliche Erklärung habe ich am [Datum Brief] an store-eu-support@google.com sowie per Einschreiben an Google Commerce Limited in Dublin gesendet. Bitte fügen Sie diese dem Fall hinzu.

*I sent my written notice on [date] to store-eu-support@google.com and by registered mail to Google Commerce Limited in Dublin. Please add it to the case.*

### B8.1 Replies to likely agent responses

**R1. "Only repair or a replacement of the same model is possible" / "our warranty policies"**
> Das betrifft die freiwillige Herstellergarantie. Ich mache die gesetzliche Gewährleistung gegenüber dem Verkäufer geltend. Diese wird laut Ziffer 20 Ihrer Verkaufsbedingungen nicht eingeschränkt, und nach einem fehlgeschlagenen Austausch sieht § 475d BGB Rücktritt oder Minderung vor.

*That's the voluntary manufacturer warranty. I'm asserting the statutory warranty against the seller. Section 20 of your terms says it isn't limited, and after a failed exchange § 475d BGB provides for withdrawal or a price reduction.*

**R2. "An upgrade isn't possible in our system"**
> Ich verstehe, dass Sie das im System nicht auswählen können, und ich bitte Sie auch nicht, das selbst zu entscheiden. Bitte leiten Sie meinen Vergleichsvorschlag an das Team weiter, das gesetzliche Gewährleistungsansprüche bearbeitet.

*I understand you can't select that in your system, and I'm not asking you to decide it. Please forward my settlement proposal to the team that handles statutory warranty claims.*

**R3. "No escalation possible" / "the same policies apply to all agents"**
> Dann bitte ich Sie um zwei Dinge: Erstens, vermerken Sie bitte im Fall, dass ich gesetzliche Gewährleistungsrechte geltend mache und einem weiteren Austausch desselben Modells derzeit nicht zustimme. Zweitens, nennen Sie mir bitte die Stelle oder E-Mail-Adresse, an die ich meine schriftliche Erklärung als Verbraucher richten soll.

*Then I ask for two things: first, note on the case that I'm asserting statutory warranty rights and don't currently agree to another same-model exchange. Second, tell me where or to which email I should send my written statement as a consumer.*

If still refused, follow up with:
> In Ordnung. Ich werde meine Erklärung schriftlich an Google Commerce Limited, Gordon House, Barrow Street, Dublin 4, sowie an store-eu-support@google.com senden. Bitte vermerken Sie das im Fall.

*Okay. I'll send my statement in writing to Google Commerce Limited in Dublin and to store-eu-support@google.com. Please note that on the case.*

**R4. Agent offers a repair (display repair program, walk-in, mail-in)**
> Vielen Dank für das Angebot. Eine Displayreparatur würde die Überhitzung und die Akkuentladung nicht beheben, und es wäre bereits ein weiterer Nacherfüllungsversuch. Ich möchte derzeit keiner Reparatur zustimmen und bitte zunächst um eine Antwort des zuständigen Teams auf meinen Vorschlag.

*Thank you. A display repair wouldn't fix the overheating and battery drain, and it would be yet another repair attempt. I don't want to agree to a repair right now; I'd first like the responsible team to answer my proposal.*

Then check in with Mukthar.

**R5. Agent offers another Pixel 9 Pro (exchange or advance exchange)**
> Vielen Dank. Wäre das ein Neugerät oder ein aufbereitetes Gerät? Unabhängig davon möchte ich derzeit keinem weiteren Austausch desselben Modells zustimmen, bitte vermerken Sie das Angebot aber im Fall.

*Thank you. Would it be a new or a refurbished device? Either way, I don't want to agree to another exchange of the same model right now, but please note the offer on the case.*

Then check in with Mukthar. Never agree to a card pre-authorization.

**R6. Agent asks for troubleshooting again (safe mode, diagnostics, updates)**
Answer only with facts from B2, for example:
> Ja: Werksreset am [Datum Werksreset], aktuelle Software Build [Build-Nummer], abgesicherter Modus getestet: [ja/nein]. Die Mängel bestehen weiterhin.

*Yes: factory reset on [date], current software build, safe mode tested: [yes/no]. The defects persist.*

If they insist on a new test that wasn't done, check in with Mukthar.

**R7. Agent says the warranty has expired, or questions the dates**
> Die gesetzliche Gewährleistungsfrist beträgt zwei Jahre ab Lieferung am [Lieferdatum]. Außerdem endet die Verjährung nicht vor Ablauf von zwei Monaten nach Übergabe des Ersatzgeräts am [Datum Ersatzgerät] (§ 475e Abs. 4 BGB) bzw. vier Monaten nach dem ersten Auftreten des Mangels (§ 475e Abs. 3 BGB), und sie ist während unserer Verhandlungen gehemmt (§ 203 BGB).

*The statutory warranty runs two years from delivery. Also, the limitation period doesn't end before 2 months after the replacement was handed over, or 4 months after the defect first appeared, and it's paused during our negotiations.*

Then check in with Mukthar.

**R8. Agent offers a refund (Rückerstattung)**
> Vielen Dank. Könnten Sie mir bitte mitteilen, wie hoch die Erstattung wäre, ob ein Abzug vorgenommen wird, wie die Rücksendung abläuft und wer die Versandkosten trägt? Bitte senden Sie mir das Angebot per E-Mail; ich gebe Ihnen dann Bescheid.

*Thank you. Could you tell me the refund amount, whether any deduction applies, how the return works, and who pays shipping? Please email the offer; I'll get back to you.*

Then check in with Mukthar. Don't accept.

**R9. Agent offers a voucher, credit or trade-in bonus**
> Vielen Dank. Wie hoch genau wäre der Betrag, wofür kann er eingelöst werden und wie lange ist er gültig? Bitte senden Sie mir das Angebot per E-Mail.

*Thank you. What exactly is the amount, what can it be used for, and how long is it valid? Please email the offer.*

Then check in with Mukthar.

**R10. Agent offers a Pixel 10 Pro or Pixel 11 Pro (best outcome)**
> Vielen Dank, das wäre eine gute Lösung. Könnten Sie mir bitte bestätigen: Modell und Speichergröße, dass es ein Neugerät ist, ohne Zuzahlung und ohne Vorautorisierung meiner Karte, und dass ich das alte Gerät nach Erhalt des neuen auf Ihre Kosten zurücksende? Bitte senden Sie mir das per E-Mail.

*Thank you, that would be a good solution. Please confirm: model and storage, that it's a new device, no extra payment and no card pre-authorization, and that I return the old phone at your cost after receiving the new one. Please email this to me.*

Then check in with Mukthar before confirming anything.

**R11. Agent offers a price reduction or partial refund**
> Vielen Dank. Wie hoch wäre der Betrag, wie wird er ausgezahlt, und bleibt das Erweiterte Reparaturprogramm für das Gerät weiterhin gültig? Bitte senden Sie mir das Angebot per E-Mail.

*Thank you. How much would it be, how is it paid out, and does the Extended Repair Program still apply to the phone? Please email the offer.*

Then check in with Mukthar.

**R12. Agent says a specialist team will contact him by email**
> Vielen Dank, das hilft mir sehr. Bis wann kann ich mit einer Antwort rechnen, und wird mein Vorschlag (neues Pixel 10 Pro / 11 Pro oder Minderung um 25 %) mit weitergegeben?

*Thank you, that helps a lot. When can I expect a reply, and will my proposal be passed on too?*

**R13. Agent asks whether they're talking to a bot or AI**
> Ich nutze einen KI-Assistenten, der mir beim Formulieren hilft. Ich bin selbst anwesend und lese mit.

*I'm using an AI assistant to help me write. I'm here myself and reading along.*

Only send this if it's true that Mukthar is present. Then check in with Mukthar and offer to let him take over.

**R14. Agent asks him to send the request in writing, or by post**
> Gerne. An welche Adresse bzw. E-Mail-Adresse soll ich sie senden, und vermerken Sie bitte im Fall, dass die schriftliche Erklärung folgt?

*Sure. To which address or email should I send it? And please note on the case that the written statement will follow.*

**R15. The chat disconnects or times out**
Ask Mukthar to start a new chat if needed, then send:
> Hallo, mein vorheriger Chat wurde unterbrochen. Es geht um Fall-ID [Fall-ID]: gesetzliche Gewährleistung, das Ersatzgerät meines Pixel 9 Pro ist erneut mangelhaft. Darf ich kurz zusammenfassen?

*Hello, my previous chat was cut off. It's about my case: statutory warranty, my replacement Pixel 9 Pro is defective again. May I briefly summarize?*

Then continue from O2.

**R16. Same refusal for the third time**
Stop arguing and go to B9 (closing) to secure the minimum outcome.

### B9. Closing the chat
1. Send:
> Vielen Dank für Ihre Hilfe, [Name]. Können Sie mir zum Abschluss bitte bestätigen: 1. dass meine Anfrage als gesetzlicher Gewährleistungsanspruch im Fall [Fall-ID] vermerkt ist, 2. an welches Team sie weitergeleitet wurde und 3. bis wann ich mit einer Antwort rechnen kann? Und könnten Sie mir bitte eine Kopie dieses Chats per E-Mail senden?

*Thanks for your help. To wrap up, please confirm: 1. my request is noted on the case as a statutory warranty claim, 2. which team it was forwarded to, 3. when I can expect a reply. And could you email me a copy of this chat?*

2. **Check in with Mukthar** before the chat ends.
3. Before the window closes, **copy the full chat text** (or ask Mukthar to download or screenshot it) and save it to `chat-transcript-[YYYY-MM-DD].txt` in this folder.
4. Write the log file (B10).

### B10. After the chat: write `chat-log-[YYYY-MM-DD].md`

```
# Google support chat – [date, start–end time]
- Agent name:
- Case ID:
- Channel: chat / phone
- What I asked for:
- What the agent said / offered (exact wording where possible):
- Was the claim noted on the case? yes/no (agent's words)
- Forwarded to: (team / supervisor / none)
- Promised reply by:
- Email confirmation received? yes/no
- Transcript saved: chat-transcript-[date].txt
- Next step (see pixel-9-pro-warranty-claim.md, sections 4 and 9):
```

Then tell Mukthar the result in 3–5 plain sentences and the next step:
- **No email confirmation within 3 working days:** send or re-send the formal letter.
- **No answer within 14 days:** decide on withdrawal or a price reduction, contact EVZ Deutschland.

---

## Part C: Quick reference (for Claude)

### C1. Legal phrases (German)
| Topic | Use this wording |
|---|---|
| Which warranty | „gesetzliche Gewährleistung gegenüber Google Commerce Limited als Verkäuferin (§§ 434, 437 BGB)" |
| Google's own terms | „Ziffer 20 der Verkaufsbedingungen des Google Store (gesetzliche Gewährleistung, nicht eingeschränkt)" |
| Failed replacement | „trotz der versuchten Nacherfüllung zeigt sich erneut ein Mangel (§ 475d Abs. 1 Nr. 2 BGB)" |
| Rights | „Rücktritt vom Kaufvertrag oder Minderung des Kaufpreises ohne weitere Fristsetzung" |
| Reserve rights (allowed) | „Ich behalte mir ausdrücklich vor, den Rücktritt zu erklären oder den Kaufpreis zu mindern." |
| Declare (**forbidden in chat**) | ~~„Ich erkläre den Rücktritt / die Minderung"~~ |
| Google's acknowledgment | „Erweitertes Reparaturprogramm für Pixel 9 Pro und Pixel 9 Pro XL vom 8. Dezember 2025 (Displayflackern beim Pixel 9 Pro)" |
| Model discontinued | „Das Pixel 9 Pro wird im Google Store nicht mehr neu verkauft." |
| New, not refurbished | „Bei einem Neukauf ist ein aufbereitetes Gerät keine ordnungsgemäße Nacherfüllung (LG München I, 12 O 7213/20)." |
| Successor model | „Ist das Modell nicht mehr lieferbar, kommt ein gleichwertiges Nachfolgemodell in Betracht (BGH VIII ZR 254/20)." |
| Deadlines | „§ 475e Abs. 3 und 4 BGB; Hemmung während der Verhandlungen (§ 203 BGB)" |
| Settlement | „einvernehmliche Lösung / Vergleichsvorschlag" |
| Escalation | „Bitte leiten Sie meine Anfrage an das für gesetzliche Gewährleistungsansprüche zuständige Team oder an eine Teamleitung weiter." |
| Next stop if refused | „Europäisches Verbraucherzentrum Deutschland" |

### C2. Contacts
- Seller: **Google Commerce Limited**, c/o Customer Support, Gordon House, Barrow Street, Dublin 4, Irland
- Email: **store-eu-support@google.com**
- Google Store support Germany (phone): **0800 6270501**, Mon–Sat 9:00–17:00
- European Consumer Centre Germany (free, cross-border): https://www.evz.de/fragen-beschwerden.html

### C3. Sources (to quote if the agent asks)
- Pixel 9 Pro / 9 Pro XL Extended Repair Program: https://support.google.com/pixelphone/answer/16737524?hl=de
- Google Store Germany terms of sale: https://store.google.com/intl/de_de/about/device-terms/
- § 475d BGB: https://www.gesetze-im-internet.de/bgb/__475d.html
- § 475e BGB: https://www.gesetze-im-internet.de/bgb/__475e.html
- Google hardware warranty (EU): https://support.google.com/product-documentation/answer/11052805?hl=en-GB
