# Architecture

## Next App

artitektur folder:

```md
├── public
|   ├── data/*
|   └── assets
|       ├── image/*
|       └── icon/*
|
├── package.json
├── package-lock.json
├── README.md
└── src
    ├── app
    |   ├── {pages}
    |   |    └── page.tsx
    |   ├── global.css
    |   ├── loading.tsx
    |   ├── not-found.tsx
    |   ├── page.tsx
    |   └── favicon.ico
    |
    ├── components
    |   ├── ui
    |   |   └── {more component library}
    |   |
    |   ├── custom
    |   |   └── {nameComponent}
    |   |        ├── {name}Iu.tsx
    |   |        └── logic
    |   |            └── {name}Logic.ts/tsx
    |   | 
    |   ├── layout
    |   |   └── {name}Layout.tsx
    |   |
    |   └── widget
    |       └── {name}Widget.tsx
    |  
    ├── hooks 
    |   └── use{Name}Hook.ts/tsx
    |
    ├── lib
    |   ├── constants.ts
    |   └── auth.ts
    └── model/*
  
```

catatan:

* `public/`: folder root data static
  * `public/assets` folder root assets `image`& `icon` static
  * `public/data` folder data json static
* `src`: root program coding
  * `src/app` halaman path awal `/`
  * `src/app/{pages}/page.tsx` file halaman browser
  * `src/components` folder root tempat komponent
    * `ui` folder component pihak ketiga (opsional)
    * `costum` folder component yang dibangun
    * `layout` folder layout (opsional)
    * `widget` berisi komponent yang lebih komplek seperti form,diagram
  * `hooks` tempat global hood disimpan seperti tema
  * `lib` berisi fungsi utilitas kecil seperti penformatan dan autentifikasi
  * `model` berisi interface data (opsional hanya di perlukan jika mengunakan ts)
