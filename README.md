# libmol

> Molecular viewer intended at undergraduate level

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

ToDo
- i18n (csv to json locales ?)
- Fichiers : legacy PDB files (ngl)
- Commandes : 
    ..- icons
    ..- labels
    ..- surface
    ..- on hover select, highlight selection
- Séquences : 
    ..- Etiquette: amino acids and nucleotide names !
    ..- 2 way binding
    ..- select from sequence
    ..- clear selection
    ..- horizontal scrolling for chains
    ..- virtual scrolling for large datasets
- display :
    ..- status bar
        ....- legendes couleurs, selection
        ....- filename
        ....- hovered atom
    ..- toolbar
        ....- fullscreen
        ....- screen capture (export)
