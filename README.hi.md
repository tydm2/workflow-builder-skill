# workflow-builder

[English](./README.md) · [简体中文](./README.zh-CN.md) · [हिन्दी](./README.hi.md) · [Português](./README.pt.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · [Deutsch](./README.de.md) · [Français](./README.fr.md) · [Русский](./README.ru.md) · [한국어](./README.ko.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](./CHANGELOG.md)
[![100%25 AI-crafted](https://img.shields.io/badge/100%25-AI--crafted-9cf.svg)](#disclaimer)

**एक डोमेन के विचार को चलाने-के-लिए-तैयार मल्टी-एजेंट वर्कफ़्लो में बदलें — 1 ऑर्केस्ट्रेटर दिमाग़ + N विशेषज्ञ subagents + एक-वाक्य ट्रिगर।**

`workflow-builder` एक एजेंट स्किल है जो एक ही डोमेन आवश्यकता से फ़ाइल-आधारित मल्टी-एजेंट पाइपलाइन का ढाँचा तैयार करता है: एक planner दिमाग़, विशेषज्ञ subagents, प्रति-एजेंट knowledge bases, स्पष्ट handoff contracts, और एक security gate — ताकि कोई भी एजेंट होस्ट आउटपुट को लोड करके तुरंत काम शुरू कर सके।

## यह क्यों ख़ास है

अधिकांश मल्टी-एजेंट टेम्प्लेट "यह रही एक भूमिका और एक प्रॉम्प्ट" पर ही रुक जाते हैं। यह स्किल पाँच विशिष्टताओं के साथ आगे बढ़ता है:

- **🔒 Security gate (सबसे मुख्य)** — एक स्वतंत्र डिलीवरी गेट जो हर उत्पन्न `AGENT.md` और knowledge फ़ाइल की समीक्षा prompt injection, दुर्भावनापूर्ण निर्देश, data exfiltration, supply-chain poisoning, और platform safety के लिए करता है, *साथ ही एक स्वतंत्र दूसरी समीक्षा*। कम्युनिटी टेम्प्लेट शायद ही कभी अपने द्वारा उत्पन्न चीज़ों की जाँच करते हैं।
- **🧠 स्वयं-विकसित होने वाले subagents** — हर subagent एक **self-iteration protocol** (feedback-log + usage-log + 5-Why retrospective + contract freeze) के साथ आता है, ताकि उत्पन्न एजेंट वास्तविक उपयोग से लगातार बेहतर होता रहे, न कि एक जमे हुए प्रॉम्प्ट बनकर रह जाए।
- **👥 विशेषज्ञ-स्तर के एजेंट, आपका निर्णय** — हर विशेषज्ञ या तो एक **expert panel** (1 लीड + 2–4 वरिष्ठ भूमिकाएँ, negotiation mechanism के साथ) है या एक **एकल वरिष्ठ विशेषज्ञ**; चुनाव साक्ष्य-आधारित है (पेपर / उच्च-स्टार रिपोज़ / कम्युनिटी सहमति) और *आप* तय करते हैं — कभी कोई डिफ़ॉल्ट नहीं।
- **🔌 प्लेटफ़ॉर्म-अनुकूल** — `AGENT.md` (DSH), `AGENTS.md` (Codex CLI), या `.claude/agents/<name>.md` (Claude Code) उत्पन्न करता है, प्रति-प्लेटफ़ॉर्म टूल मैपिंग के साथ, ताकि एक ही डिज़ाइन हर होस्ट पर काम करे।
- **♻️ Blueprint पुनर्उपयोग + ADR** — पूर्ण हुए वर्कफ़्लो Architecture Decision Records के साथ पुन: उपयोग योग्य blueprints के रूप में संग्रहीत किए जाते हैं, और वर्कफ़्लो स्वयं उपयोग की प्रतिक्रिया से विकसित होता है।

साथ ही: वैकल्पिक **community skill research** (स्रोत सुरक्षित रखते हुए सर्वोत्तम कम्युनिटी स्किल्स का निचोड़), **create + edit दोहरा मोड**, और फ़ाइल अनुबंधों के लिए एक **single source of truth**।

## यह कैसे काम करता है — 8 चरण

1. **स्पष्ट करें (Clarify)** — डोमेन, उपयोग मोड (नया / संपादन / दोनों), चरण, गुणवत्ता की लाल रेखाएँ, knowledge की ताज़गी, कम्युनिटी रिसर्च, ट्रिगर शब्द, और लक्षित प्लेटफ़ॉर्म पर विकल्प-आधारित प्रश्न।
2. **Community research (वैकल्पिक)** — शीर्ष कम्युनिटी स्किल्स खोजें, पुन: उपयोग योग्य हिस्सों का निचोड़ निकालें, स्रोत सुरक्षित रखें, सुरक्षा समीक्षा चलाएँ।
3. **टोपोलॉजी डिज़ाइन करें** — 1 दिमाग़ + 2–4 विशेषज्ञ; आप panel बनाम एकल-वरिष्ठ-विशेषज्ञ चुनें; प्रति-विशेषज्ञ create/edit निर्णय।
4. **ढाँचा तैयार करें (Scaffold)** — charter टेम्प्लेट से `agents/<name>/AGENT.md` + `knowledge/` उत्पन्न करें (variable तालिका प्रति एजेंट भरी जाए)।
5. **Knowledge bases भरें** — अंतर्निर्मित (ऑफ़लाइन) और ताज़ा करने योग्य (search-first, "recent updates" अनुभाग के साथ)।
6. **पाइपलाइन जोड़ें** — handoff contracts, README पाइपलाइन आरेख, ट्रिगर-शब्द रजिस्ट्री, वर्कफ़्लो-स्तरीय लॉग, blueprint संग्रह।
7. **स्वीकार करें और वितरित करें** — कागज़ी वॉकथ्रू **फिर पहला end-to-end smoke run**; ट्री, ट्रिगर और पहले रन के कमांड रिपोर्ट करें।
8. **Security gate** — पाँच सुरक्षा मदों के लिए हर charter और knowledge फ़ाइल की पूर्ण समीक्षा, साथ ही एक स्वतंत्र दूसरा पास।

## आउटपुट

```
your-workflow/
  README.md                  # pipeline आरेख + trigger रजिस्ट्री + ADR + runtime iteration protocol
  shared/                    # क्रॉस-एजेंट लाइब्रेरीज़
  agents/<name>/AGENT.md     # charter: पहचान, प्रोटोकॉल, गुणवत्ता की लाल रेखाएँ, self-iteration
  agents/<name>/knowledge/   # अंतर्निर्मित और ताज़ा करने योग्य knowledge bases
  blueprints/<domain>.md     # पुन: उपयोग योग्य topology + ADR निर्णय रिकॉर्ड
  feedback-log.md / usage-log.md  # वर्कफ़्लो-स्तरीय self-evolution
  <stage>/                   # प्रति चरण संस्करणबद्ध आर्टिफ़ैक्ट
```

## इंस्टॉल

```
~/.dsh/skills/workflow-builder/    # वैश्विक (global)
.dsh/skills/workflow-builder/      # प्रति प्रोजेक्ट
```

फिर इसे ऐसे वाक्यांशों से शुरू करें जैसे *"build me a <domain> workflow"*, *"set up a plan→execute pipeline"*, *"assemble a subagent team"* — या **set-skill** के `/skill` मेनू आइटम ④ के माध्यम से।

## उदाहरण

- `references/example-novel-mode.md` — उपन्यास-लेखन की तीन-एजेंट पाइपलाइन (Planner → Outliner → Writer)।
- `examples/deep-research-pipeline/` — पूर्ण charters और knowledge bases के साथ एक स्व-निर्मित deep-research पाइपलाइन (Planner → Researcher → Writer → Reviewer)।

## दस्तावेज़

- `references/pipeline-design.md` — topology पद्धति, expert-form चयन, knowledge विभाजन, community research और safety review
- `references/agent-charter-template.md` — AGENT.md मानक टेम्प्लेट
- `references/prompt-craft.md` — पेशेवर subagent प्रॉम्प्ट-लेखन विशिष्टता
- `references/platform-adapter.md` — DSH / Codex CLI / Claude Code मैपिंग
- `references/contract-spec.md` — फ़ाइल अनुबंधों के लिए single source of truth
- `references/blueprint-reuse.md` — blueprint संग्रह और पुनर्उपयोग, ADR, वर्कफ़्लो-स्तरीय runtime iteration

## सहयोगी स्किल

यह स्किल **[set-skill](https://github.com/tydm2/create-generate-skill)** के साथ काम करने के लिए डिज़ाइन किया गया है — जो स्किल बनाने और ऑडिट करने की meta-skill है। `set-skill` का `/skill` मेनू यहाँ आइटम ④ के रूप में रूट होता है, और `workflow-builder` subagent self-evolution के लिए `set-skill` के feedback-log / usage-log / contract-freeze तंत्रों का पुन: उपयोग करता है।

## आवश्यकताएँ

- एक एजेंट होस्ट जो subagents चला सके और फ़ाइलें पढ़ सके — DSH मूल रूप से; Codex CLI / Claude Code adapter के माध्यम से।
- कम्युनिटी रिसर्च के लिए वेब सर्च (वैकल्पिक; अनुपलब्ध होने पर सहजता से घट जाता है)।

## अस्वीकरण

> **यह स्किल 100% AI-निर्मित है।** समस्याएँ अपरिहार्य हैं — चर्चा और pull requests का स्वागत है। लेखक वास्तविक-दुनिया उपयोग के आधार पर इसे सक्रिय रूप से बेहतर बनाता रहता है, और समय के साथ इसे परिष्कृत करता रहेगा।

## लाइसेंस

[MIT](./LICENSE)
