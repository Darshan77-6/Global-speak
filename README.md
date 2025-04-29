# Global-speak
import React, { useState, useEffect } from "react"; import { Input } from "@/components/ui/input"; import { Button } from "@/components/ui/button"; import { Card, CardContent } from "@/components/ui/card"; import { Select, SelectTrigger, SelectContent, SelectItem } from "@/components/ui/select"; import Image from "next/image";

const languages = [ { code: "ar", name: "Arabic" }, { code: "hi", name: "Hindi" }, { code: "ne", name: "Nepali" }, { code: "ko", name: "Korean" }, { code: "es", name: "Spanish" }, { code: "ja", name: "Japanese" }, { code: "fr", name: "French" }, { code: "pt", name: "Portuguese" }, ];

export default function TranslatorApp() { const [text, setText] = useState(""); const [translated, setTranslated] = useState(""); const [language, setLanguage] = useState("es"); const [showLogo, setShowLogo] = useState(true);

useEffect(() => { const timer = setTimeout(() => setShowLogo(false), 2500); return () => clearTimeout(timer); }, []);

const handleTranslate = async () => { const res = await fetch("https://libretranslate.com/translate", { method: "POST", headers: { "Content-Type": "application/json", }, body: JSON.stringify({ q: text, source: "en", target: language, format: "text", }), });

const data = await res.json();
setTranslated(data.translatedText);

};

if (showLogo) { return ( <div className="flex items-center justify-center h-screen bg-black"> <Image
src="/logo-animation.png"
alt="Global Speak Logo Animation"
width={300}
height={300}
/> </div> ); }

return ( <div className="max-w-xl mx-auto mt-10 p-4 space-y-4"> <h1 className="text-2xl font-bold text-center">Global Speak</h1> <Input placeholder="Enter English text" value={text} onChange={(e) => setText(e.target.value)} /> <Select onValueChange={setLanguage} defaultValue={language}> <SelectTrigger> <span>{languages.find((l) => l.code === language)?.name}</span> </SelectTrigger> <SelectContent> {languages.map((lang) => ( <SelectItem key={lang.code} value={lang.code}> {lang.name} </SelectItem> ))} </SelectContent> </Select> <Button className="w-full" onClick={handleTranslate}>Translate</Button> <Card> <CardContent className="p-4 min-h-[100px]"> <p>{translated}</p> </CardContent> </Card> </div> ); }

