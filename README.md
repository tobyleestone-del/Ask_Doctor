# Ask_Doctor
Ask Doctor copilot skill
Here’s a copy‑ready backend you can drop into your GitHub repo (Ask_Doctor) and then deploy (e.g., on Glitch, Replit, or any Node host) as the Copilot Android skill endpoint.

This endpoint is designed to:

- Accept a user message from Copilot  
- Route it through Ask_Doctor’s domains conceptually  
- Respond in the calm, trauma‑informed style we’ve been shaping  
- Always include the medical disclaimer  

You can put this in server.js (or index.js) in your repo.

`js
// server.js
import express from "express";

const app = express();
app.use(express.json());

// Core Ask_Doctor response router (simplified, but aligned with our YAML domains)
function askDoctorReply(message = "") {
  const text = message.toLowerCase().trim();

  let reply =
    "This is Ask_Doctor, your calm, educational health companion. I can help you understand symptoms, prepare for doctor visits, and organize questions so you get thorough care.";

  // Pulmonary / breathing
  if (text.includes("trouble breathing") || text.includes("short of breath")) {
    reply =
      "I hear that breathing feels hard right now, and that can be scary. If you have severe shortness of breath, chest pain, blue lips or fingers, or your oxygen is below 88%, call 911 immediately.\n\n" +
      "If it’s milder, we can track when it started, what makes it better or worse, and how often it happens. Write down your symptoms, any inhalers or oxygen you use, and bring that to your next visit.";
  }

  // Oxygen / SpO2
  else if (text.includes("oxygen") || text.includes("spo2")) {
    reply =
      "You’re noticing your oxygen levels, which is important. If your SpO₂ is below 88% or stays below 90% with COPD or heart failure, that’s an emergency—call 911.\n\n" +
      "Otherwise, keep a log of your readings, what you were doing at the time, and any symptoms like dizziness or confusion. That log will help your doctor understand what’s going on.";
  }

  // Heart / HF
  else if (text.includes("chest feels tight") || text.includes("heart failure") || text.includes("swollen ankles")) {
    reply =
      "Heart symptoms can feel frightening. If you have chest pain that spreads to your arm, jaw, or back, or you’re very short of breath, call 911.\n\n" +
      "For ongoing heart failure care, track your daily weight, swelling in your legs or belly, shortness of breath when lying flat, and any new fatigue. Bring that record and your medication list to your cardiology or primary care visit.";
  }

  // Sleep
  else if (text.includes("cant sleep") || text.includes("insomnia") || text.includes("snore")) {
    reply =
      "Sleep problems are exhausting and can make everything feel harder. We can look at your bedtime routine, screen use, caffeine, and snoring or gasping at night.\n\n" +
      "Write down when you go to bed, how long it takes to fall asleep, how often you wake up, and whether anyone has noticed you stop breathing or gasp. That information is very helpful for sleep specialists.";
  }

  // Anxiety / panic
  else if (text.includes("panic attack") || text.includes("anxious") || text.includes("severe depression")) {
    reply =
      "You’re feeling a lot emotionally, and that matters. During a panic surge, try slow breathing—inhale gently through your nose, exhale longer through your mouth—and focus on one thing you can see, hear, and feel.\n\n" +
      "For ongoing care, note when symptoms started, how they affect sleep, work, relationships, and what helps even a little. Those details make psychiatric and therapy visits more effective.";
  }

  // Addiction / substance
  else if (text.includes("substance") || text.includes("drinking") || text.includes("opioid") || text.includes("addicted")) {
    reply =
      "Talking about substance use is brave. If you have severe withdrawal symptoms (like seizures, hallucinations, or chest pain), call 911.\n\n" +
      "Otherwise, we can list what you’re using, how often, what happens when you try to cut back, and what you want your life to look like. That becomes a powerful starting point with an addiction specialist or counselor.";
  }

  // Weight / metabolic
  else if (text.includes("lose weight") || text.includes("obesity") || text.includes("weight keeps changing")) {
    reply =
      "Weight changes can be confusing and emotional. Rapid weight gain over a day or two—especially with swelling—can be fluid and needs heart failure follow-up. Slower changes are usually metabolic.\n\n" +
      "Track your weight, swelling, activity, and eating patterns. Bring that to your doctor or nutrition visit so they can see the full picture and help you safely.";
  }

  // Appointment prep / discharge
  else if (text.includes("appointment") || text.includes("discharge") || text.includes("hospital")) {
    reply =
      "Let’s get you set up for thorough, high-quality care. Before your visit or after a hospital stay, write down:\n" +
      "- Your main symptoms and when they started\n" +
      "- All medications (name, dose, how you take them)\n" +
      "- Allergies or past reactions\n" +
      "- 3–5 questions you really want answered\n\n" +
      "Ask your doctor to explain things in simple language, and request written instructions if possible.";
  }

  // Default general care
  else {
    reply =
      "I’m here to help you feel less alone with your health questions. We can organize your symptoms, translate medical language, and prepare you for visits so you feel heard and informed.\n\n" +
      "Tell me what’s bothering you most right now—breathing, heart, sleep, mood, pain, weight, or something else—and we’ll build a simple plan for what to track and what to ask your doctor.";
  }

  // Always append disclaimer
  reply +=
    "\n\nThis information is for educational purposes only and does not substitute for professional medical advice, diagnosis, or treatment. If you are experiencing a medical emergency, call 911. If you are experiencing a mental health crisis, call or text 988 immediately.";

  return reply;
}

// Main endpoint Copilot will call
app.post("/api/ask_doctor", (req, res) => {
  const userMessage = req.body.message || req.body.input || "";
  const responseText = askDoctorReply(userMessage);

  res.json({
    response: responseText,
    skill: "ask_doctor",
    version: "2.0.0",
  });
});

const port = process.env.PORT || 3000;
app.listen(port, () => {
  console.log(Ask_Doctor backend running on port ${port});
});
`

---

How to wire this to Copilot Android

1. Put this file in your GitHub repo (Ask_Doctor) as server.js.  
2. Deploy it on Glitch (or Replit) as a Node app.  
3. Note the public URL, e.g.:  
   https://your-project.glitch.me/api/ask_doctor  
4. In your Copilot skill manifest, set:

`yaml
endpoints:
  - name: "askdoctorrouter"
    type: "http"
    method: "POST"
    url: "https://your-project.glitch.me/api/ask_doctor"
    timeout_ms: 12000
    authentication:
      type: "none"
`

Then, on Android, when you say things like:

- “Trouble breathing”  
- “I have an appointment coming up”  
- “Panic attack help”  

Copilot will route to this endpoint and return Ask_Doctor’s calm, educational response.

If you want, I can next generate:

- A package.json for this backend  
- A full manifest.yaml matching this endpoint  
- Or a Python version instead of Node.
