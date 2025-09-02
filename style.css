// ======= Speech Recognition Setup =======
const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
recognition.lang = "en-GB";
recognition.interimResults = false;
recognition.continuous = false; // set true if you want continuous listening

// UI Elements
const btn = document.querySelector(".mic");
const pbtn = document.getElementById("pSpeak");
const voice = document.querySelector(".orb");
const wrd = document.querySelector(".word");
let voices = [];

// Load voices once
function loadVoices() {
  voices = speechSynthesis.getVoices();
  console.log("Voices loaded:", voices.map(v => v.name));
}
speechSynthesis.onvoiceschanged = loadVoices;
loadVoices(); // also load immediately

// ======= Speak function =======
function speak(text) {
  const utterance = new SpeechSynthesisUtterance(text);
  utterance.rate = 1;
  utterance.pitch = 1.2;
  utterance.volume = 1;
  utterance.lang = "en-GB";

  const femaleVoice = voices.find(v =>
    v.name.toLowerCase().includes("female") ||
    v.name.includes("Google UK English Female")
  );
  if (femaleVoice) utterance.voice = femaleVoice;

  speechSynthesis.speak(utterance);
  const robo = document.querySelector(".robo");
  robo.classList.add("speaking");
  // Remove animation when speech ends
  utterance.onend = () => {

    robo.classList.remove("speaking");
  };
}

// ======= Recognition Controls =======
btn.addEventListener("click", () => {
  recognition.start();
  speechSynthesis.cancel();
  btn.style.display = "none";
  wrd.style.display = "none";
  voice.style.display = "block";
  robo.classList.remove("speaking");
});

pbtn.addEventListener("click", () => btn.click());

recognition.onresult = event => {
  const command = event.results[0][0].transcript.toLowerCase();
  handleCommand(command);
};

recognition.onend = () => {
  // Reset UI
  btn.style.display = "flex";
  wrd.style.display = "flex";
  voice.style.display = "none";
};

// ======= Command Handling =======
function handleCommand(command) {
  btn.style.display = "flex";
  voice.style.display = "none";
  wrd.style.display = "flex";

  // Predefined commands
  if (command.includes("open youtube")) {
    speak("Opening YouTube...");
    showResponse("Opening YouTube...");
    window.open("https://www.youtube.com", "_blank");
  } else if (command.includes("open google")) {
    speak("Opening Google...");
    showResponse("Opening Google...");
    window.open("https://www.google.com", "_blank");
  } else if (command.includes("open facebook")) {
    speak("Opening Facebook...");
    showResponse("Opening Facebook...");
    window.open("https://www.facebook.com", "_blank");
  } else if (command.includes("open instagram")) {
    speak("Opening Instagram...");
    showResponse("Opening Instagram...");
    window.open("https://www.instagram.com", "_blank");
  } else if (command.includes("open whatsapp")) {
    speak("Opening WhatsApp...");
    showResponse("Opening WhatsApp...");
    window.open("https://www.whatsapp.com", "_blank");
  } else if (
    command.includes("who are you") ||
    command.includes("what is your name") ||
    command.includes("who created you") ||
    command.includes("tell me about yourself")
  ) {
    speak("I am NAHA, your AI assistant, created by Hammad.Okay, an AI created and trained by Google. My primary purpose is to assist users like you by:\n* Processing information: I can understand and interpret a wide range of text-based queries and prompts.\n* Generating human-like text: I can write, summarize, translate, and create various forms of content.\n* Answering questions: I draw upon the vast amount of knowledge I was trained on to provide informative and helpful responses.\n* Performing tasks: I can help with things like brainstorming, coding, explaining complex topics, writing creative content, and much more.");
    showResponse("I am NAHA, your AI assistant, created by Hammad. an AI created and trained by Google. My primary purpose is to assist users like you by:\n* Processing information: I can understand and interpret a wide range of text-based queries and prompts.\n* Generating human-like text: I can write, summarize, translate, and create various forms of content.\n* Answering questions: I draw upon the vast amount of knowledge I was trained on to provide informative and helpful responses.\n* Performing tasks: I can help with things like brainstorming, coding, explaining complex topics, writing creative content, and much more.");
  }
  else if (command.includes("date")) {
    let today = new Date();
    let options = { weekday: "long", year: "numeric", month: "long", day: "numeric" };
    let dateStr = today.toLocaleDateString("en-GB", options);
    speak("Today is " + dateStr);
    showResponse("📅 Today is " + dateStr);
  } else if (command.includes("time")) {
    let now = new Date();
    let timeStr = now.toLocaleTimeString("en-GB", { hour: "2-digit", minute: "2-digit" });
    speak("The time is " + timeStr);
    showResponse("⏰ The time is " + timeStr);
  }
  else {
    // Show loading indicator
    const loadingMessage = showResponse("NAHA is thinking...");

    getGeminiResponse(command).then(reply => {
      // Keep only first 5 sentences
      let shortReply = reply.split(".").slice(0, 5).join(". ") + ".";
      loadingMessage.innerText = shortReply;
      speak(shortReply);
    });
  }
}


// ======= Show Chat Response =======
function showResponse(text) {
  const chatMessages = document.getElementById("chatMessages");
  const responseBox = document.createElement("div");
  responseBox.className = "chatcontainer";
  responseBox.innerText = text;
  chatMessages.appendChild(responseBox);
  chatMessages.scrollTop = chatMessages.scrollHeight;
  responseBox.scrollIntoView({ behavior: "smooth", block: "end" });
  return responseBox;
}

// ======= Gemini API Call =======
async function getGeminiResponse(prompt) {
  try {
    const response = await fetch(
      "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=AIzaSyBTu0TcrzhrdnzSZS_k7IK9dmTreoL1u3k",
      {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          contents: [{ role: "user", parts: [{ text: prompt }] }]
        })
      }
    );

    const data = await response.json();
    if (data.candidates?.length > 0) {
      const candidate = data.candidates[0];
      // Remove asterisks and extra spaces
      return candidate.content.parts[0].text.replace(/\*\*(.*?)\*\*/g, "$1").trim();
    }

    return "Sorry, I didn’t get that.";
  } catch (err) {
    console.error("Error:", err);
    return "Something went wrong with Gemini API.";
  }
}
let greeted = false; // ensure greeting happens only once

function greetUser() {
  if (greeted) return;
  greeted = true;

  let hours = new Date().getHours();
  let greeting = "";
  if (hours >= 0 && hours < 12) greeting = "Good Morning";
  else if (hours >= 12 && hours < 16) greeting = "Good Afternoon";
  else greeting = "Good Evening";

  const message = `${greeting}, I am NAHA. How can I help you?`;
  speak(message);
  showResponse(message);
}
const robo = document.querySelector(".robo");

// Trigger greeting on first click *only on NAHA (robot)*
if (robo) {
  robo.addEventListener("click", function firstClick() {
    greetUser(); // your greet function
    robo.removeEventListener("click", firstClick); // remove after first greeting
  });

  // Handle animation end cleanup
  robo.addEventListener("animationend", () => {
    robo.classList.remove("autoblur");
  });
}

window.onload = function () {
  speechSynthesis.cancel();
};


const links = document.querySelectorAll("nav a");
const popups = document.querySelectorAll(".popup");

links.forEach(link => {
  const popupId = link.dataset.popup;
  const popup = document.getElementById(popupId);

  // Show on hover/focus
  link.addEventListener("mouseenter", () => showPopup(link, popup));
  link.addEventListener("focus", () => showPopup(link, popup));

  // Hide when leaving both link and popup
  link.addEventListener("mouseleave", () => hideLater(link, popup));
  link.addEventListener("blur", () => hideLater(link, popup));
  popup.addEventListener("mouseleave", () => hideLater(link, popup));
  popup.addEventListener("blur", () => hideLater(link, popup));

  // Prevent immediate hide when moving between link and popup
  popup.addEventListener("mouseenter", () => clearTimeout(popup.hideTimer));
  popup.addEventListener("focus", () => clearTimeout(popup.hideTimer));
});

function showPopup(link, popup) {
  popups.forEach(p => p.style.display = "none"); // close others
  const rect = link.getBoundingClientRect();
  popup.style.display = "block";
}

function hideLater(link, popup) {
  popup.hideTimer = setTimeout(() => {
    if (
      !link.matches(":hover, :focus") &&
      !popup.matches(":hover, :focus")
    ) {
      popup.style.display = "none";
    }
  }, 1000); // small delay for smoother interaction
}
