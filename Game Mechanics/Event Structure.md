# Event Structure & Design

**Last Updated**: 2025-11-03

This document details the branching event system that creates interactive, multi-layered encounters where different crew skills matter at different decision points.


---

## Core Event Philosophy

Events move beyond simple one-choice-one-outcome structures to create:

- **Question Phase**: Gather information before committing to actions
- **Branching Complications**: Initial choices lead to new problems requiring different skills
- **Failure States**: Failed actions create alternative paths, not just dead ends
- **Skill Diversity**: No single crew skill solves everything
- **Crew Variance**: Different crew members provide different advantages

---

## Event Structure Phases

### Phase 1: Discovery
Initial encounter presents the situation and establishes stakes.

**Elements:**
- Event title and type classification
- Initial scenario description
- Immediate observations
- Available information gathering options

**Example:**
```
EVENT: The Silent Station
TYPE: Exploration

Your crew approaches an abandoned research station. 
All lights are off. No response to hails. 
Docking ports are accessible.

What do you want to know?
[8-12 questions available based on crew skills]
```

---

### Phase 2: Question Phase
Crew members can answer questions based on their skills. Each answer provides useful information but doesn't commit to action.

**Design Principles:**
- 8-12 questions per event minimum
- The player can pick 3 questions to ask
- Multiple characters can answer same question (different perspectives)
- Specialists give better/more accurate information
- Some questions require specific character presence
- Questions requiring specific characters or specific skills are not available to choose.
- Information gathering has no Strain cost

**Question Types:**

**Investigation Questions:**
- "What can you learn from the exterior?"
- "Are there signs of recent activity?"
- "What's the station's condition?"

**Technical Questions:**
- "Can you access their systems remotely?"
- "What's their power status?"
- "Are life support systems functional?"

**Science Questions:**
- "Any biological hazards detectable?"
- "What was this station researching?"
- "Environmental readings?"

**Social Questions:**
- "What would cause this silence?"
- "Who operated this station?"
- "Any distress signals in history?"

**Medical Questions:**
- "Signs of medical emergency?"
- "Contamination risks?"
- "What could kill everyone?"

**Example Question Format:**
```json
{
  "question": "What can you learn from exterior scans?",
  "skill_required": "investigation",
  "answers": {
    "rex": "Hull intact. No battle damage. But... airlocks 
            been opened recently. From inside.",
    "ghost": "Someone left in a hurry. Docking clamp 
             damage suggests emergency departure. 
             One ship, maybe two weeks ago.",
    "viktor": "Tactical assessment: No defensive damage. 
               Whatever happened, they weren't attacked 
               from outside."
  }
}
```

---

### Phase 3: Primary Action
After gathering information, crew chooses main approach. Each action requires specific skills and leads to different complication branches.

**Action Categories:**

**Direct Investigation:**
- Requires: Investigation, Science, or Medical
- Leads to: Discovery complications
- Risk level: Medium

**Technical Approach:**
- Requires: Technical, Stealth
- Leads to: System complications
- Risk level: Low-Medium

**Social/Communication:**
- Requires: Social, Investigation
- Leads to: Interaction complications
- Risk level: Variable

**Combat/Tactical:**
- Requires: Combat, Stealth
- Leads to: Confrontation complications
- Risk level: High

**Example Primary Actions:**
```
1. INVESTIGATE THE LABS
   Skills: Science, Investigation, Medical
   → Success: Find research data
   → Partial: Find data, trigger something
   → Failure: Trigger containment breach

2. HACK THE SYSTEMS
   Skills: Technical, Investigation
   → Success: Full access, crew locations
   → Partial: Partial access, security triggered
   → Failure: Lockdown activated

3. SEARCH FOR SURVIVORS
   Skills: Medical, Social, Investigation
   → Success: Find survivors, learn truth
   → Partial: Find survivors, complicated
   → Failure: Find bodies, trigger defenses
```

---

### Phase 4: Complications
Primary action succeeds or fails, leading to a **new problem** requiring different skills. This prevents one character from solving entire event.

**Complication Design Principles:**
- Success leads to one complication
- Failure leads to different complication
- Complications require skills NOT used in primary action
- Multiple solutions possible for each complication
- Adds Strain based on severity

**Example Complication Chain:**

**If "Hack Systems" Succeeds:**
```
COMPLICATION: System Access Granted
"You're in, but the station AI is... wrong. 
It's asking questions that don't make sense. 
Responding incorrectly might trigger lockdown."

NEW OPTIONS:
1. COMMUNICATE WITH AI [Social + Science]
   → Requires Sarah or Magdalene best
   
2. MANUALLY OVERRIDE SAFETY [Technical + Combat]
   → Requires Wrench or Viktor
   
3. INTERPRET AI CORRUPTION [Science + Investigation]
   → Requires Elena or Yuki
```

**If "Hack Systems" Fails:**
```
COMPLICATION: Security Lockdown
"Alarms blare. Sections seal. You have 
minutes before station security systems 
activate fully. You need OUT."

NEW OPTIONS:
1. FORCE EMERGENCY OVERRIDE [Combat + Technical]
   → Requires Viktor or Boom
   
2. FIND MAINTENANCE CRAWLSPACE [Stealth + Investigation]
   → Requires Ghost or Patches
   
3. PILOT EMERGENCY DETACH [Piloting + Technical]
   → Requires Rex or Boom
```

---

### Phase 5: Resolution
Final outcome based on complication handling. Multiple ending states possible.

**Resolution Tiers:**

**Optimal Resolution:**
- Primary action succeeded
- Complication handled well
- All objectives achieved
- Minimal Strain
- Rewards gained

**Good Resolution:**
- Primary or complication succeeded
- Most objectives achieved
- Moderate Strain
- Some rewards

**Partial Resolution:**
- Mixed success/failure
- Some objectives achieved
- Significant Strain
- Limited rewards

**Poor Resolution:**
- Primary and complication failed
- Few objectives achieved
- Heavy Strain
- Minimal rewards

**Catastrophic Resolution:**
- Everything failed
- No objectives achieved
- Massive Strain
- Possible casualties/damage

---

## Event Templates with Strain Integration

### Template 1: Exploration Event

**THE ABANDONED OUTPOST**

**Discovery Phase:**
```
An abandoned mining outpost detected.
No life signs. Valuable equipment possibly inside.
```

**Question Phase (10 questions):**
- Exterior condition [Investigation]
- Power systems [Technical]
- Last transmissions [Social/Investigation]
- Environmental hazards [Science]
- Recent activity [Investigation/Stealth]
- Structural integrity [Technical]
- Medical evacuation signs [Medical]
- Corporate ownership [Social/Investigation]
- Ore storage status [Science/Technical]
- Defensive systems [Combat/Technical]

**Primary Actions:**

**1. INVESTIGATE CAREFULLY**
- Skills: Investigation (3), Science (2)
- Base Strain: +0
- Success: Find valuable data
- Failure: Trigger automated defenses

**2. SALVAGE EQUIPMENT FAST**
- Skills: Technical (3), Stealth (2)
- Base Strain: +1 (rushed)
- Success: Recover valuable gear
- Failure: Equipment damaged

**3. ACCESS COMPUTERS**
- Skills: Technical (4)
- Base Strain: +0
- Success: Learn what happened
- Failure: Security lockdown

**Complications:**

*If Investigation Succeeds:*
**COMPLICATION: Contamination Detected**
- Crew exposed to unknown spores
- New skills needed: Medical (4) or Science (3)
- Character-specific:
  - Elena: -1 Strain (research opportunity)
  - Patches: +0 Strain (medical challenge)
  - Others: +2 Strain (contamination fear)

*If Investigation Fails:*
**COMPLICATION: Defense Systems Active**
- Combat drones deploying
- New skills needed: Combat (3), Stealth (4), or Technical (3)
- Base Strain: +3 (life-threatening)
- Character-specific:
  - Viktor/Boom: +0 Strain (their specialty)
  - Ghost: +2 Strain (forced into combat)
  - Academics: +4 Strain (not trained for this)

**Resolution Outcomes:**

**Optimal:**
- Strain: Investigation success -2, contamination handled -1
- Rewards: +500 credits, research data
- All crew: Net -2 Strain

**Catastrophic:**
- Strain: +3 defense drones, +4 crew injured
- Damage: Hull -10
- Patches: +6 Strain (medical crisis)
- All crew: Net +5-7 Strain

---

### Template 2: Social Event

**PIRATE NEGOTIATION**

**Discovery Phase:**
```
Pirates intercept your ship. They're not 
attacking yet, but weapons are armed.
They want your cargo.
```

**Question Phase (12 questions):**
- Pirate faction identification [Investigation/Social]
- Ship capabilities assessment [Combat/Piloting]
- Cargo value estimate [Investigation]
- Crew composition scan [Social/Investigation]
- Escape route analysis [Piloting]
- Weapon systems comparison [Combat/Technical]
- Communication protocols [Social]
- Bluff opportunities [Social/Investigation]
- Historical encounters [Social]
- Legal jurisdiction [Social/Investigation]
- Technical vulnerabilities [Technical/Combat]
- Medical readiness [Medical]

**Primary Actions:**

**1. NEGOTIATE PEACEFULLY**
- Skills: Social (4)
- Base Strain: +0
- Character-specific:
  - Sarah: +0 (her job)
  - Johnny: +0 (loves this)
  - Iris: +0 (corporate negotiator)
  - Others: +1 (tense)
- Success: Reach agreement
- Failure: Demands increase

**2. INTIMIDATE THEM**
- Skills: Combat (3), Social (2)
- Base Strain: +1
- Character-specific:
  - Rex/Viktor: +0 (experienced)
  - Others: +2 (dangerous gambit)
- Success: Pirates back off
- Failure: Combat triggered

**3. PREPARE FOR COMBAT**
- Skills: Combat (3), Technical (2), Piloting (2)
- Base Strain: +2 (combat stress)
- Character-specific:
  - Combat crew: +0
  - Academic crew: +3
- Success: Advantageous position
- Failure: They shoot first

**Complications:**

*If Negotiation Succeeds:*
**COMPLICATION: They Want Crew Member**
- Pirates recognize someone (randomly selected)
- New skills needed: Social (5) or Combat (4)
- Character-specific Strain:
  - Recognized character: +6 (personal threat)
  - Ghost: +8 if recognized (worst fear)
  - Rex: +5 if crew threatened
  - Others: +3 (crew in danger)

*If Negotiation Fails:*
**COMPLICATION: Combat Initiated**
- Ship-to-ship combat begins
- New skills needed: Piloting (4), Combat (3)
- Base Strain: +3
- Character-specific:
  - Sarah: +5 (failed her job)
  - Johnny: +4 (negotiation failure)
  - Combat crew: +1 (expected)

**Resolution Outcomes:**

**Optimal (Negotiation → Protected Crew):**
- Sarah/Johnny: -4 Strain (successful negotiation)
- All crew: -2 Strain (avoided violence)
- Net: Most crew -2 to -4 Strain

**Poor (Failed Negotiation → Combat → Casualties):**
- Sarah: +5 failed negotiation, +3 casualties = +8 total
- Patches: +6 Strain (medical failure)
- All crew: +3 Strain (loss)
- Close friends: +5 Strain (grief)
- Net: +8-11 Strain for most crew

---

### Template 3: Crisis Event

**HULL BREACH**

**Discovery Phase:**
```
CRITICAL ALERT: Hull breach detected.
Section sealing. Crew members trapped.
Life support failing in 10 minutes.
```

**Question Phase (8 questions):**
- Breach location/size [Technical/Investigation]
- Trapped crew status [Medical]
- Structural integrity [Technical]
- Seal options [Technical]
- Rescue routes [Investigation/Stealth]
- Time estimate [Technical/Medical]
- Equipment availability [Technical]
- Environmental hazards [Science]

**Primary Actions:**

**1. EMERGENCY REPAIR**
- Skills: Technical (5), Combat (2)
- Base Strain: +3 (life-threatening)
- Time pressure: Requires speed
- Character-specific:
  - Wrench: +0 (her job)
  - Boom: +1 (structural work)
  - Magdalene: +1 (machine spirits help)
  - Others: +4 (not their expertise)
- Success: Breach sealed
- Failure: Breach worsens

**2. RESCUE TRAPPED CREW**
- Skills: Medical (3), Combat (3), Stealth (2)
- Base Strain: +3 (danger)
- Character-specific:
  - Patches/Viktor: +1 (trained for this)
  - Others: +3 (rescue anxiety)
- Success: Everyone saved
- Failure: Casualties

**3. ABANDON SECTION**
- Skills: Technical (3), Piloting (2)
- Base Strain: +2
- Character-specific:
  - Any trapped crew: +8 (survival guilt)
  - Rex: +6 (abandoned crew)
  - Others: +4 (terrible choice)
- Success: Ship saved, crew lost
- Failure: Breach spreads anyway

**Complications:**

*If Repair Succeeds:*
**COMPLICATION: Sealed But Power Failing**
- Life support still critical
- New skills needed: Technical (3) + Science (2)
- Base Strain: +2 (ongoing crisis)
- Character-specific:
  - Magdalene: +0 (machine communion)
  - Wrench: +1 (another problem)
  - Others: +2 (still dangerous)

*If Repair Fails:*
**COMPLICATION: Catastrophic Systems Failure**
- Multiple breaches cascading
- New skills needed: Piloting (5) + Combat (3)
- Base Strain: +5 (ship-wide emergency)
- Character-specific:
  - Rex: +2 (his worst fear manifesting)
  - All crew: +5 (near-death)

**Resolution Outcomes:**

**Optimal (Repair → Power Restored → All Safe):**
- Lead engineer: -4 Strain (impossible fix)
- All crew: -3 Strain (relief)
- Net: Everyone -3 to -4 Strain

**Catastrophic (Repair Fails → Casualties → Ship Damaged):**
- Lead engineer: +5 Strain (failure)
- Patches: +6 Strain (medical failure)
- All crew: +5 Strain (near-death)
- Close friends: +5 additional (grief)
- Hull: -30 (major damage)
- Net: Everyone +10-16 Strain

---

## Event Design Checklist

When creating new events, verify:

### Structure
- [ ] Discovery phase establishes situation clearly
- [ ] 8-12 questions with multiple character answers
- [ ] 3-5 primary action options
- [ ] Each action requires different skill combinations
- [ ] Success/failure lead to different complications
- [ ] Complications require skills NOT used in primary action
- [ ] Multiple resolution outcomes possible

### Balance
- [ ] Base Strain appropriate for danger level
- [ ] Character-specific triggers identified
- [ ] Success modifiers included (-1 to -2)
- [ ] Achievement opportunities present (-2 to -4)
- [ ] Mission outcome modifiers defined
- [ ] Strain totals follow 70/20/10 distribution guideline

### Skill Coverage
- [ ] No single character can solve entire event
- [ ] Multiple skills matter across phases
- [ ] Specialists get moment to shine
- [ ] Generalists can contribute
- [ ] Failure creates new opportunities (not dead ends)

### Character Relevance
- [ ] Personal triggers matter for some characters
- [ ] Achievement opportunities for some characters
- [ ] Personality-appropriate responses
- [ ] Relationship dynamics can matter
- [ ] Different crews will experience event differently

---

## Implementation Notes

### Displaying Questions
```
Available crew can answer questions.
Show character name + skill rating:

"What can you learn from scans?"
├─ Rex [Investigation 2]: Basic info
├─ Ghost [Investigation 3]: Detailed info
└─ Viktor [Investigation 1]: Limited info
```

### Tracking Choices
```
Event state should track:
- Questions asked (can ask all or select few)
- Primary action taken
- Success/failure result
- Complication encountered
- Resolution outcome
- Strain changes per character
- Rewards/consequences
```

### Procedural Variation
```
To increase replayability:
- Randomize some complications
- Vary trapped crew members
- Adjust skill DCs based on crew composition
- Change specific details (pirate faction, station type)
- Keep core structure, change flavor
```

---

## Tags
#mechanics #events #structure #design #branching