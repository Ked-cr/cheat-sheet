Framer Motion, sebuah library animasi untuk React yang sangat populer karena sintaksnya simpel dan powerful. Kalau kamu mau belajar, aku kasih step-by-step biar gampang diikuti 👇

## 1. Install Framer Motion

Kalau kamu pakai Next.js atau React biasa:
```
npm install framer-motion
```

## 2. Animasi dasar (fade in & fade out)

Contoh animasi masuk/keluar elemen:
```
"use client";
import { motion } from "framer-motion";

export default function Example() {
  return (
    <motion.div
      initial={{ opacity: 0 }}       // keadaan awal
      animate={{ opacity: 1 }}       // keadaan setelah animasi
      exit={{ opacity: 0 }}          // saat keluar
      transition={{ duration: 1 }}   // durasi animasi
      className="p-6 bg-blue-500 text-white rounded-lg"
    >
      Hello Motion 👋
    </motion.div>
  );
}
```
## 3. Variants (lebih rapih)

Kalau animasi banyak, biasanya dipisah pakai variants:
```
const boxVariant = {
  hidden: { opacity: 0, y: 50 },
  visible: { opacity: 1, y: 0 }
};

<motion.div
  variants={boxVariant}
  initial="hidden"
  animate="visible"
  transition={{ duration: 0.5 }}
>
  Variants Example 🚀
</motion.div>
```
## 4. Animasi Hover & Tap

Bisa bikin interaksi langsung:
```
<motion.button
  whileHover={{ scale: 1.1 }}
  whileTap={{ scale: 0.9 }}
  className="px-4 py-2 bg-green-600 text-white rounded-lg"
>
  Klik Aku
</motion.button>
```
## 5. AnimatePresence (animasi keluar-masuk komponen)

Kalau pakai conditional rendering:
```
"use client";
import { motion, AnimatePresence } from "framer-motion";
import { useState } from "react";

export default function ToggleBox() {
  const [show, setShow] = useState(true);

  return (
    <div className="flex flex-col items-center gap-4">
      <button
        onClick={() => setShow(!show)}
        className="px-4 py-2 bg-indigo-600 text-white rounded-lg"
      >
        Toggle
      </button>

      <AnimatePresence>
        {show && (
          <motion.div
            initial={{ opacity: 0, y: 50 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -50 }}
            transition={{ duration: 0.5 }}
            className="w-40 h-40 bg-indigo-400 rounded-lg"
          />
        )}
      </AnimatePresence>
    </div>
  );
}
```
## stats

- Dasar → initial, animate, exit

- Interaksi → whileHover, whileTap

- Organisasi → variants

- Kompleks → AnimatePresence, useAnimation, scroll-based animation