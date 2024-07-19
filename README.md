# Usage

Install dependencies

```sh
yarn
```

Open project in dev mode

```sh
yarn dev
```

# Customize

## Change font-family

You can choose your prefer font from google font and set it simply changing font-family in `theme.css`

```css
/* theme.css */
html {
  color-scheme: light;
  font-family: 'Inter';
}
```

Optionally, you can configure it from `nuxt.config.ts`

```ts
  ...
  googleFonts: {
    families: {
      Roboto: true,
      'Josefin+Sans': true,
      Lato: [400, 600],
      Raleway: {
        wght: [100, 400],
        ital: [100]
      },
      Inter: '200..700',
      'Crimson Pro': {
        wght: '200..900',
        ital: '200..700',
      }
    }
  },

```

## Change schema color

1. Add your theme to `theme.css`

```css
.theme-myTheme {
    --background: 0 0% 100%;
    --foreground: 224 71.4% 4.1%;

    --muted: 220 14.3% 95.9%;
    --muted-foreground: 220 8.9% 46.1%;

    --popover: 0 0% 100%;
    --popover-foreground: 224 71.4% 4.1%;

    --card: 0 0% 100%;
    --card-foreground: 224 71.4% 4.1%;

    --border: 220 13% 91%;
    --input: 220 13% 91%;

    --primary: 158.6 100% 43.1%;
    --primary-foreground: 210 20% 98%;

    --secondary: 220 14.3% 95.9%;
    --secondary-foreground: 220.9 39.3% 11%;

    --accent: 220 14.3% 95.9%;
    --accent-foreground: 220.9 39.3% 11%;

    --destructive: 0 84.2% 60.2%;
    --destructive-foreground: 210 20% 98%;

    --ring: 262.1 83.3% 57.8%;

    --radius: 0.5rem;
}

.theme-myTheme.dark {
    --background: 222.24 48.2% 11.2%;
    --foreground: 210 20% 98%;

    --muted: 215 27.9% 16.9%;
    --muted-foreground: 217.9 10.6% 64.9%;

    --popover: 222.24 48.2% 11.2%;
    --popover-foreground: 210 20% 98%;

    --card: 222.24 48.2% 11.2%;
    --card-foreground: 210 20% 98%;

    --border: 215 27.9% 16.9%;
    --input: 215 27.9% 16.9%;

    --primary: 158.6 100% 43.1%;
    --primary-foreground: 210 20% 98%;

    --secondary: 215 27.9% 16.9%;
    --secondary-foreground: 210 20% 98%;

    --accent: 215 27.9% 16.9%;
    --accent-foreground: 210 20% 98%;

    --destructive: 0 62.8% 30.6%;
    --destructive-foreground: 210 20% 98%;

    --ring: 263.4 70% 50.4%;
}
```

Add theme options to `theme.ts`

```ts

...
  {
    name: 'myTheme',
    label: 'myTheme',
    activeColor: {
      light: '158.6 100% 43.1%',
      dark: '158.6 100% 43.1%',
    },
    cssVars: {
      light: {
        'background': '0 0% 100%',
        'foreground': '224 71.4% 4.1%',
        'card': '0 0% 100%',
        'card-foreground': '224 71.4% 4.1%',
        'popover': '0 0% 100%',
        'popover-foreground': '224 71.4% 4.1%',
        'primary': '262.1 83.3% 57.8%',
        'primary-foreground': '210 20% 98%',
        'secondary': '220 14.3% 95.9%',
        'secondary-foreground': '220.9 39.3% 11%',
        'muted': '220 14.3% 95.9%',
        'muted-foreground': '220 8.9% 46.1%',
        'accent': '220 14.3% 95.9%',
        'accent-foreground': '220.9 39.3% 11%',
        'destructive': '0 84.2% 60.2%',
        'destructive-foreground': '210 20% 98%',
        'border': '220 13% 91%',
        'input': '220 13% 91%',
        'ring': '262.1 83.3% 57.8%',
      },
      dark: {
        'background': '222.24 48.2% 11.2%',
        'foreground': '210 20% 98%',
        'card': '222.24 48.2% 11.2%',
        'card-foreground': '210 20% 98%',
        'popover': '222.24 48.2% 11.2%',
        'popover-foreground': '210 20% 98%',
        'primary': '158.6 100% 43.1%',
        'primary-foreground': '210 20% 98%',
        'secondary': '215 27.9% 16.9%',
        'secondary-foreground': '210 20% 98%',
        'muted': '215 27.9% 16.9%',
        'muted-foreground': '217.9 10.6% 64.9%',
        'accent': '215 27.9% 16.9%',
        'accent-foreground': '210 20% 98%',
        'destructive': '0 62.8% 30.6%',
        'destructive-foreground': '210 20% 98%',
        'border': '215 27.9% 16.9%',
        'input': '215 27.9% 16.9%',
        'ring': '263.4 70% 50.4%'
      }

    },
```

3. From `ThemeCustomizer.vue` add your theme to `allColors`

```ts

// Create an array of color values
const allColors: Color[] = [
  'zinc',
  'rose',
  'blue',
  'green',
  'orange',
  'red',
  'slate',
  'stone',
  'gray',
  'neutral',
  'yellow',
  'myTheme'
];

```
