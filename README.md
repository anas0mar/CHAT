import React, { useState, useEffect } from "react";
import { initializeApp } from "firebase/app";
import { getAuth, RecaptchaVerifier, signInWithPhoneNumber, onAuthStateChanged } from "firebase/auth";
import { getDatabase, ref, push, onValue } from "firebase/database";
import PhoneInput from "react-phone-input-2";
import "react-phone-input-2/lib/style.css";
import { useTranslation } from "react-i18next";
import i18n from "./i18n";
import i18n from "i18next";
import { initReactI18next } from "react-i18next";

const resources = {
  en: {
    translation: {
      change_language: "Change Language",
      send_otp: "Send OTP",
      enter_otp: "Enter OTP",
      verify_otp: "Verify OTP",
      chat: "Chat",
      type_message: "Type a message...",
      send: "Send",
      otp_sent: "OTP has been sent!",
    },
  },
  ar: {
    translation: {
      change_language: "تغيير اللغة",
      send_otp: "إرسال رمز التحقق",
      enter_otp: "أدخل رمز التحقق",
      verify_otp: "تحقق من الرمز",
      chat: "الدردشة",
      type_message: "اكتب رسالة...",
      send: "إرسال",
      otp_sent: "تم إرسال رمز التحقق!",
    },
  },
};

i18n.use(initReactI18next).init({
  resources,
  lng: "en",
  fallbackLng: "en",
  interpolation: {
    escapeValue: false,
  },
});

export default i18n;


// Firebase Configuration
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const database = getDatabase(app);

export default function ChatApp() {
  const { t } = useTranslation();
  const [phone, setPhone] = useState("");
  const [otp, setOtp] = useState("");
  const [user, setUser] = useState(null);
  const [message, setMessage] = useState("");
  const [messages, setMessages] = useState([]);

  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, (authenticatedUser) => {
      setUser(authenticatedUser);
    });
    return () => unsubscribe();
  }, []);

  useEffect(() => {
    if (user) {
      const messagesRef = ref(database, "messages");
      onValue(messagesRef, (snapshot) => {
        const data = snapshot.val();
        if (data) {
          setMessages(Object.values(data));
        }
      });
    }
  }, [user]);

  const sendOtp = () => {
    if (!window.recaptchaVerifier) {
      window.recaptchaVerifier = new RecaptchaVerifier(auth, "recaptcha-container", { size: "invisible" });
    }
    const appVerifier = window.recaptchaVerifier;
    signInWithPhoneNumber(auth, `+${phone}`, appVerifier)
      .then((confirmationResult) => {
        window.confirmationResult = confirmationResult;
        alert(t("otp_sent"));
      })
      .catch((error) => console.error("OTP Error:", error));
  };

  const verifyOtp = () => {
    if (window.confirmationResult) {
      window.confirmationResult.confirm(otp)
        .then((result) => {
          setUser(result.user);
        })
        .catch((error) => console.error("OTP Verification Error:", error));
    }
  };

  const sendMessage = () => {
    if (message && user) {
      push(ref(database, "messages"), { text: message, sender: user.phoneNumber || "Unknown" });
      setMessage("");
    }
  };

  return (
    <div className="p-4 max-w-md mx-auto">
      <button onClick={() => i18n.changeLanguage(i18n.language === "en" ? "ar" : "en")} className="bg-gray-500 text-white p-2 w-full mb-2">
        {t("change_language")}
      </button>
      {!user ? (
        <div>
          <PhoneInput
            country={"us"}
            value={phone}
            onChange={setPhone}
            inputClass="border p-2 w-full"
          />
          <button onClick={sendOtp} className="bg-green-500 text-white p-2 w-full mt-2">{t("send_otp")}</button>
          <div id="recaptcha-container"></div>
          <input 
            type="text" 
            placeholder={t("enter_otp")} 
            value={otp} 
            onChange={(e) => setOtp(e.target.value)}
            className="border p-2 w-full mt-2" 
          />
          <button onClick={verifyOtp} className="bg-blue-500 text-white p-2 w-full mt-2">{t("verify_otp")}</button>
        </div>
      ) : (
        <div>
          <h2 className="text-lg font-bold">{t("chat")}</h2>
          <div className="border p-4 h-64 overflow-auto">
            {messages.map((msg, index) => (
              <div key={index} className="p-2 border-b">
                <strong>{msg.sender}: </strong>{msg.text}
              </div>
            ))}
          </div>
          <input 
            type="text" 
            placeholder={t("type_message")} 
            value={message} 
            onChange={(e) => setMessage(e.target.value)}
            className="border p-2 w-full mt-2" 
          />
          <button onClick={sendMessage} className="bg-green-500 text-white p-2 w-full mt-2">{t("send")}</button>
        </div>
      )}
    </div>
  );
}
