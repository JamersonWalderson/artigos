# Stylesheets Puros no Expo: Da Tentativa com Ignite à Simplicidade Inspirada no Bluesky

## Uma análise técnica de 7 meses de desenvolvimento — incluindo o que foi tentado e abandonado

---

## Introdução: o problema inicial

Em **19 de julho de 2025**, iniciei o desenvolvimento do **Econolista**, um aplicativo de lista de compras cross-platform (web, Android e iOS). O Expo foi escolhido pela promessa de unificar essas plataformas com uma única base de código.

O desafio imediato: como garantir **responsividade** e **consistência visual** entre plataformas sem depender de bibliotecas de UI que poderiam trazer instabilidade?

Antes de encontrar a solução final, passei por uma jornada de tentativas — incluindo o boilerplate **Ignite** da Infinite Red e padrões do **Bluesky Social App**.

---

## A tentativa com Ignite: por que abandonei

### O que é o Ignite?

O [Ignite](https://github.com/infinitered/ignite) é o boilerplate React Native mais antigo e popular, mantido pela [Infinite Red](https://infinite.red/) desde 2016. Promete economizar 2-4 semanas de setup inicial com:

- Estrutura de pastas opinativa
- Sistema de temas com cores, tipografia e espaçamento
- Geradores de componentes e modelos
- Configuração de navegação pronta
- Suporte a internacionalização (i18n)
- Integração com Reactotron para debugging

### A estrutura do Ignite

```
your-project/
├── app/
│   ├── components/      # Componentes reutilizáveis
│   ├── config/          # Configurações por ambiente
│   ├── devtools/        # Reactotron e ferramentas de dev
│   ├── i18n/            # Internacionalização
│   ├── context/         # React Context providers
│   ├── navigators/      # React Navigation
│   ├── screens/         # Telas da aplicação
│   ├── services/        # APIs e serviços externos
│   ├── theme/           # Cores, tipografia, espaçamento
│   ├── utils/           # Utilitários diversos
│   └── app.tsx          # Entry point
├── assets/
├── ignite/
│   └── templates/       # Templates para geradores
├── test/
└── ...
```

### O sistema de temas do Ignite

O Ignite traz um sistema de temas sofisticado com arquivos separados:

**`app/theme/spacing.ts`** (código original do Ignite):
```typescript
export const spacing = {
  xxxs: 2,
  xxs: 4,
  xs: 8,
  sm: 12,
  md: 16,
  lg: 24,
  xl: 32,
  xxl: 48,
  xxxl: 64,
} as const
```

**`app/theme/colors.ts`** (código original do Ignite):
```typescript
const palette = {
  neutral100: "#FFFFFF",
  neutral200: "#F4F2F1",
  neutral300: "#D7CEC9",
  // ... mais cores
  primary500: "#C76542",
  angry500: "#C03403",
} as const

export const colors = {
  palette,
  transparent: "rgba(0, 0, 0, 0)",
  text: palette.neutral800,
  textDim: palette.neutral600,
  background: palette.neutral200,
  border: palette.neutral400,
  tint: palette.primary500,
  error: palette.angry500,
  // ...
} as const
```

**Hook `useAppTheme`** do Ignite:
```typescript
const $container: ThemedStyle<ViewStyle> = (theme) => ({
  flex: 1,
  backgroundColor: theme.colors.background,
  justifyContent: "center",
  alignItems: "center",
})

const Component = () => {
  const { themed } = useAppTheme()
  return <View style={themed($container)} />
}
```

### Por que abandonei o Ignite

Após experimentar o boilerplate, identifiquei problemas que me fizeram desistir:

#### 1. Complexidade excessiva para o propósito

O Econolista é um app de lista de compras — não um sistema empresarial. O Ignite traz:
- **i18n** — Não precisava de internacionalização
- **Reactotron** — Debugging avançado desnecessário para MVP
- **Geradores CLI** — Overhead para um projeto pequeno
- **MST (MobX State Tree)** — Gerenciamento de estado complexo demais

#### 2. Tempo perdido em decisões secundárias

Passei horas decidindo:
- Qual estrutura de pastas seguir exatamente
- Como adaptar os geradores para meu caso
- Se deveria usar MST ou outra solução de estado
- Como configurar o Reactotron corretamente

Essas decisões **não eram as mais importantes** para entregar valor ao usuário.

#### 3. Curva de aprendizado do boilerplate

O Ignite tem suas próprias convenções. Aprender o "jeito Ignite" de fazer as coisas tomou tempo que poderia ser investido no produto.

#### 4. Dificuldade de customização

Quando precisei mudar algo que o Ignite fazia de um jeito específico, foi mais difícil do que começar do zero.

### A decisão: começar limpo

Abandonei o Ignite e criei uma aplicação limpa com `npx create-expo-app`. A partir daí, adicionei apenas o que precisava, quando precisava.

---

## O que aproveitei do Ignite (conceitos, não código)

Embora tenha abandonado o boilerplate, alguns **conceitos** do Ignite influenciaram minha arquitetura:

### 1. Separação de `services/` para APIs

**Ignite:**
```
app/services/    # APIs e serviços externos
```

**Econolista:**
```
service/
├── api.ts           # Cliente Axios
├── apiService.ts    # Classe de serviço com métodos
├── pocketbase.ts    # Cliente PocketBase
└── types.ts         # Interfaces TypeScript
```

A ideia de centralizar chamadas de API em uma pasta dedicada veio do Ignite.

### 2. Pasta `hooks/` para lógica reutilizável

**Ignite:** Não tem pasta `hooks/` explícita, mas usa hooks em `utils/`.

**Econolista:**
```
hooks/
├── useCategories.ts
├── useShoppingList.ts
├── useShoppingCartForm.ts
├── useItems.ts
├── usePWAInstall.ts
└── ...
```

Criei uma pasta dedicada para hooks customizados, inspirado na organização do Ignite.

### 3. Pasta `util/` para helpers

**Ignite:**
```
app/utils/    # Helpers e utilitários
```

**Econolista:**
```
util/
├── currency.ts      # Formatação de moeda
├── interfaces.ts    # Interfaces compartilhadas
└── routes.ts        # Constantes de rotas
```

### 4. Sistema de espaçamento com constantes

**Ignite (`spacing.ts`):**
```typescript
export const spacing = {
  xxxs: 2,
  xxs: 4,
  xs: 8,
  sm: 12,
  md: 16,
  lg: 24,
  xl: 32,
  xxl: 48,
  xxxl: 64,
} as const
```

**Econolista (`design/theme.ts`):**
```typescript
const commonSpacing = {
  xs: 4,
  sm: 8,
  md: 16,
  lg: 24,
  xl: 32,
};
```

Simplifiquei para 5 níveis em vez de 9 — suficiente para o projeto.

---

## Comparativo: Ignite vs. Econolista

| Aspecto | Ignite | Econolista |
|---------|--------|------------|
| **Estrutura de pastas** | Opinativa, completa | Mínima, cresceu conforme necessidade |
| **Sistema de temas** | Arquivos separados (colors, spacing, typography) | Arquivo único (`theme.ts`) |
| **Gerenciamento de estado** | MST (MobX State Tree) | React Context + SWR |
| **Navegação** | React Navigation configurado | Expo Router (file-based) |
| **i18n** | Incluído | Não necessário |
| **Debugging** | Reactotron | Console + React DevTools |
| **Geradores** | CLI própria | Nenhum |
| **Tempo de setup** | ~30 min (com aprendizado) | ~5 min |
| **Complexidade** | Alta | Baixa |

---

## A inspiração final: Bluesky Social App

Após abandonar o Ignite, encontrei inspiração no código-fonte do **Bluesky Social App** ([github.com/bluesky-social/social-app](https://github.com/bluesky-social/social-app)).

O Bluesky é uma aplicação React Native de produção com milhões de usuários. Seu código mostrou que é possível ter uma arquitetura elegante **sem** boilerplates pesados.

### Arquivos do Bluesky que adaptei

#### 1. `useWebMediaQueries.tsx` — Hook de media queries

**Código original do Bluesky** ([src/lib/hooks/useWebMediaQueries.tsx](https://github.com/bluesky-social/social-app/blob/main/src/lib/hooks/useWebMediaQueries.tsx)):

```typescript
import {useMediaQuery} from 'react-responsive'
import {IS_NATIVE} from '#/env'

/**
 * @deprecated use `useBreakpoints` from `#/alf` instead
 */
export function useWebMediaQueries() {
  const isDesktop = useMediaQuery({minWidth: 1300})
  const isTablet = useMediaQuery({minWidth: 800, maxWidth: 1300 - 1})
  const isMobile = useMediaQuery({maxWidth: 800 - 1})
  const isTabletOrMobile = isMobile || isTablet
  const isTabletOrDesktop = isDesktop || isTablet
  if (IS_NATIVE) {
    return {
      isMobile: true,
      isTablet: false,
      isTabletOrMobile: true,
      isTabletOrDesktop: false,
      isDesktop: false,
    }
  }
  return {isMobile, isTablet, isTabletOrMobile, isTabletOrDesktop, isDesktop}
}
```

**Minha adaptação** (`design/usewebmediaqueries.ts`):

```typescript
import { useMediaQuery } from "react-responsive";
import { isNative } from "./detection";

export function useWebMediaQueries() {
  const isDesktop = useMediaQuery({ minWidth: 1300 });
  const isTablet = useMediaQuery({ minWidth: 800, maxWidth: 1300 - 1 });
  const isMobile = useMediaQuery({ maxWidth: 800 - 1 });
  const isTabletOrMobile = isMobile || isTablet;
  const isTabletOrDesktop = isDesktop || isTablet;
  if (isNative) {
    return {
      isMobile: true,
      isTablet: false,
      isTabletOrMobile: true,
      isTabletOrDesktop: false,
      isDesktop: false,
    };
  }
  return { isMobile, isTablet, isTabletOrMobile, isTabletOrDesktop, isDesktop };
}
```

**Por que funcionou:** O padrão de retornar valores fixos para plataformas nativas evita chamadas desnecessárias de media queries em mobile.

---

#### 2. `breakpoints.ts` — Sistema de breakpoints avançado

**Código original do Bluesky** ([src/alf/breakpoints.ts](https://github.com/bluesky-social/social-app/blob/main/src/alf/breakpoints.ts)):

```typescript
import {useMemo} from 'react'
import {useMediaQuery} from 'react-responsive'

export type Breakpoint = 'gtPhone' | 'gtMobile' | 'gtTablet'

export function useBreakpoints(): Record<Breakpoint, boolean> & {
  activeBreakpoint: Breakpoint | undefined
} {
  const gtPhone = useMediaQuery({minWidth: 500})
  const gtMobile = useMediaQuery({minWidth: 800})
  const gtTablet = useMediaQuery({minWidth: 1300})
  return useMemo(() => {
    let active: Breakpoint | undefined
    if (gtTablet) {
      active = 'gtTablet'
    } else if (gtMobile) {
      active = 'gtMobile'
    } else if (gtPhone) {
      active = 'gtPhone'
    }
    return {
      activeBreakpoint: active,
      gtPhone,
      gtMobile,
      gtTablet,
    }
  }, [gtPhone, gtMobile, gtTablet])
}

export function useLayoutBreakpoints() {
  const rightNavVisible = useMediaQuery({minWidth: 1100})
  const centerColumnOffset = useMediaQuery({minWidth: 1100, maxWidth: 1300})
  const leftNavMinimal = useMediaQuery({maxWidth: 1300})

  return {
    rightNavVisible,
    centerColumnOffset,
    leftNavMinimal,
  }
}
```

**Minha adaptação** (`design/breakpoints.ts`):

```typescript
import { useMemo } from "react";
import { useMediaQuery } from "react-responsive";

export type Breakpoint = "gtPhone" | "gtMobile" | "gtTablet";

export function useBreakpoints(): Record<Breakpoint, boolean> & {
  activeBreakpoint: Breakpoint | undefined;
} {
  // Default values for server-side rendering
  const defaultValues = {
    activeBreakpoint: undefined as Breakpoint | undefined,
    gtPhone: false,
    gtMobile: false,
    gtTablet: false,
  };

  // Only use media queries on the client side
  if (typeof window === "undefined") {
    return defaultValues;
  }

  const gtPhone = useMediaQuery({ minWidth: 500 });
  const gtMobile = useMediaQuery({ minWidth: 800 });
  const gtTablet = useMediaQuery({ minWidth: 1300 });

  return useMemo(() => {
    let active: Breakpoint | undefined;
    if (gtTablet) {
      active = "gtTablet";
    } else if (gtMobile) {
      active = "gtMobile";
    } else if (gtPhone) {
      active = "gtPhone";
    }
    return {
      activeBreakpoint: active,
      gtPhone,
      gtMobile,
      gtTablet,
    };
  }, [gtPhone, gtMobile, gtTablet]);
}
```

**O que adicionei:** Suporte a SSR com verificação de `typeof window === "undefined"`.

---

#### 3. `platform.ts` — Funções de identidade por plataforma

**Minha implementação** (`design/platform.ts`):

```typescript
import { Platform } from "react-native";
import { isAndroid, isIOS, isNative, isWeb } from "./detection";

/**
 * Identity function on web. Returns nothing on other platforms.
 */
export function web(value: any) {
  if (isWeb) {
    return value;
  }
}

/**
 * Identity function on iOS. Returns nothing on other platforms.
 */
export function ios(value: any) {
  if (isIOS) {
    return value;
  }
}

/**
 * Identity function on Android. Returns nothing on other platforms.
 */
export function android(value: any) {
  if (isAndroid) {
    return value;
  }
}

/**
 * Identity function on iOS and Android. Returns nothing on web.
 */
export function native(value: any) {
  if (isNative) {
    return value;
  }
}

export const platform = Platform.select;
```

**Por que funcionou:** Permite aplicar estilos condicionalmente sem `if/else` verbosos:

```typescript
// Antes (verboso)
const style = Platform.OS === 'web' ? { cursor: 'pointer' } : {};

// Depois (limpo)
const style = web({ cursor: 'pointer' });
```

---

#### 4. `detection.ts` — Detecção de plataforma

**Minha implementação** (`design/detection.ts`):

```typescript
import { Platform } from "react-native";

export const isIOS = Platform.OS === "ios";
export const isAndroid = Platform.OS === "android";
export const isNative = isIOS || isAndroid;
export const isWeb = !isNative;
export const isMobileWebMediaQuery = "only screen and (max-width: 1300px)";
```

---

## Melhores práticas de Stylesheet encontradas

### 1. Função de estilos com tema como parâmetro

**Padrão adotado:**

```typescript
const s = (theme: Theme) =>
  StyleSheet.create({
    container: {
      backgroundColor: theme.colors.background,
      padding: theme.spacing.md,
    },
  });

// Uso no componente
const { theme } = useTheme();
<View style={s(theme).container} />
```

**Por que funcionou:** Permite que os estilos reajam dinamicamente ao tema (light/dark) sem re-renderizações desnecessárias.

---

### 2. Composição de estilos com arrays

**Padrão adotado:**

```typescript
<View
  style={[
    styles.base,
    isMobile && styles.mobile,
    isDesktop && styles.desktop,
    customStyle,
  ]}
/>
```

**Por que funcionou:** O React Native ignora valores `false` e `undefined` em arrays de estilos, permitindo composição condicional limpa.

---

### 3. Estilos inline para valores dinâmicos do tema

**Padrão adotado:**

```typescript
<View
  style={{
    backgroundColor: theme.colors.rightSurface,
    padding: theme.spacing.md,
    borderRadius: theme.borderRadius.sm,
  }}
/>
```

**Por que funcionou:** Para valores que dependem do tema atual, estilos inline são mais diretos que criar StyleSheets dinâmicos.

---

### 4. Separação de estilos por responsabilidade

**Estrutura final adotada:**

```
design/
├── breakpoints.ts        # Hooks de breakpoints (inspirado no Bluesky)
├── detection.ts          # Detecção de plataforma (inspirado no Bluesky)
├── platform.ts           # Funções de identidade (inspirado no Bluesky)
├── theme.ts              # Definição de temas (simplificado do Ignite)
├── usewebmediaqueries.ts # Hook de media queries (copiado do Bluesky)
├── index.tsx             # ThemeProvider e exports
└── Layout/
    ├── index.tsx         # Componente Layout principal
    ├── Screen.tsx        # Container de tela
    └── Header/           # Componentes de header
```

---

## O que funcionou vs. o que não funcionou

### ✅ O que funcionou

#### 1. Sistema de breakpoints do Bluesky
**Commit:** `aca1f4f` (19/07/2025)

O sistema de breakpoints com `useBreakpoints()` e `useWebMediaQueries()` funcionou perfeitamente desde o primeiro dia.

#### 2. ThemeProvider com Context API
**Commit:** `aca1f4f` (19/07/2025)

```typescript
export function ThemeProvider({ children }: { children: ReactNode }) {
  const [darkMode, setDarkMode] = useState(false);

  useEffect(() => {
    const subscription = Appearance.addChangeListener(({ colorScheme }) => {
      setDarkMode(colorScheme === "dark");
    });
    setDarkMode(Appearance.getColorScheme() === "dark");
    return () => subscription.remove();
  }, []);

  const theme = darkMode ? themes.dark : themes.light;

  return (
    <ThemeContext.Provider value={{ theme, darkMode, setDarkMode }}>
      {children}
    </ThemeContext.Provider>
  );
}
```

#### 3. Layout responsivo com Left/Right
**Commit:** `fea0089` (19/07/2025)

Componentes compostos (`Layout.Left`, `Layout.Right`) permitem layouts flexíveis sem prop drilling.

#### 4. Pasta `service/` para APIs (conceito do Ignite)

Centralizar chamadas de API em uma pasta dedicada manteve o código organizado.

#### 5. Pasta `hooks/` para lógica reutilizável

Hooks customizados como `useShoppingList`, `useCategories` e `usePWAInstall` encapsulam lógica complexa.

---

### ❌ O que não funcionou (e foi removido/simplificado)

#### 1. Boilerplate Ignite completo
**Decisão:** Abandonado antes do primeiro commit

O Ignite era complexo demais para o propósito. Perdi tempo em decisões que não eram as mais importantes.

#### 2. Parallax Scroll View
**Commits:** `303942b` → `6b7b20a` (21/07 → 17/10/2025)

```
303942b - feat(layout2): layout 2 e tentativa de parallax
0524e7b - feat(parralax): parallax funcional e comandos para build
6b7b20a - refact(layout): componente sem o parallax
```

**Por que não funcionou:**
- Performance inconsistente entre web e mobile
- Complexidade adicional sem benefício claro para UX

#### 3. Múltiplos arquivos de Header
**Commits:** `fea0089` → `c4392a5` (19/07 → 30/07/2025)

Inicialmente criei uma estrutura complexa que depois consolidei em um único arquivo.

#### 4. Configuração de Google Auth
**Commits:** Múltiplos fixes entre 10/08 e 14/08/2025

Autenticação OAuth requer testes em todas as plataformas desde o início.

---

## Cronologia completa do desenvolvimento

### Fase 1: Fundação (19-21 de julho de 2025)

| Data | Commit | Descrição | Arquivos-chave |
|------|--------|-----------|----------------|
| 19/07 | `40aa132` | Commit inicial (template Expo limpo, **não Ignite**) | Template padrão |
| 19/07 | `aca1f4f` | **Criação do sistema de design** | `design/breakpoints.ts`, `design/theme.ts`, `design/detection.ts`, `design/platform.ts` |
| 19/07 | `fea0089` | Layout responsivo com Header | `design/Layout/`, `design/usewebmediaqueries.ts` |
| 20/07 | `f4150b6` | Sistema de autenticação | `provider/session.tsx`, `components/AuthGuard.tsx` |
| 20/07 | `4de3809` | Cliente Axios | `service/api.ts` |
| 21/07 | `33475a7` | Tela de lista com layout responsivo | `app/(main)/lista.tsx` |

### Fase 2: Componentes UI (21-29 de julho de 2025)

| Data | Commit | Descrição | Arquivos-chave |
|------|--------|-----------|----------------|
| 21/07 | `d1e2c6d` | ThemedTextInput | `components/ThemedTextInput.tsx` |
| 21/07 | `82c867a` | ShoppingListItem | `components/ShoppingListItem.tsx` |
| 21/07 | `303942b` | Layout2 + tentativa parallax | `design/Layout2/index.tsx` |
| 23/07 | `4eade14` | ShoppingCartItem | `components/ShoppingCartItem.tsx` |
| 27/07 | `53fb5c2` | ThemedSearchInput | `components/ThemedSearchInput.tsx` |
| 28/07 | `3f0fec9` | Busca com autocomplete | Hooks de busca |

### Fase 3: Produção (agosto de 2025)

| Data | Commit | Descrição |
|------|--------|-----------|
| 03/08 | `0bd3933` | Drawer navigation |
| 03/08 | `fd5ce5e` | Docker setup |
| 10/08 | `c9a9017` | Versão 1.0.2 |

### Fase 4: PWA (setembro de 2025)

| Data | Commit | Descrição |
|------|--------|-----------|
| 12/09 | `2a1b6c2` | Home redesenhada |
| 13/09 | `44e24ad` | PWA com ícones |
| 16/09 | `eeedc15` | **Tag 1.0.32** — versão estável |

### Fase 5: Migração PocketBase (outubro-novembro de 2025)

| Data | Commit | Descrição |
|------|--------|-----------|
| 15/10 | `d14fe19` | Auth PocketBase |
| 17/10 | `2acfcc3` | Realtime |
| 17/10 | `6b7b20a` | **Remoção do parallax** |
| 04/11 | `d929984` | **Versão 2.0.0** |

### Fase 6: Refinamentos (novembro-dezembro de 2025)

| Data | Commit | Descrição |
|------|--------|-----------|
| 28/11 | `dcfd591` | Código de compartilhamento |
| 09/12 | `baef507` | Versão 2.0.22 (atual) |

---

## Dependências finais do projeto

```json
{
  "expo": "53.0.22",
  "react": "19.0.0",
  "react-native": "0.79.5",
  "react-responsive": "^10.0.1",
  "pocketbase": "^0.26.2",
  "axios": "^1.10.0",
  "react-hook-form": "^7.62.0",
  "swr": "^2.3.4"
}
```

**Nota:** Nenhuma biblioteca de UI como NativeBase, Tamagui ou NativeWind foi incluída. Nenhum boilerplate como Ignite foi usado. A única dependência para responsividade é `react-responsive` (4KB gzipped).

---

## Conclusão: lições aprendidas

### 1. Boilerplates podem ser armadilhas de tempo

O Ignite é excelente para equipes que já o conhecem ou projetos complexos. Para um MVP ou projeto pessoal, começar limpo e adicionar conforme necessário é mais eficiente.

### 2. Estudar código de produção vale mais que documentação

O Bluesky Social App me ensinou padrões que nenhuma documentação de biblioteca oferecia. Ver como uma equipe profissional resolve problemas reais é inestimável.

### 3. Conceitos são mais valiosos que código

Do Ignite, aproveitei **conceitos** (separação de pastas, sistema de espaçamento) sem copiar o código. Do Bluesky, adaptei **código real** que resolvia meus problemas.

### 4. Simplicidade vence complexidade

O parallax foi removido. Os múltiplos arquivos de Header foram consolidados. O Ignite foi abandonado. O que sobrou é simples, testado e funciona.

### 5. Breakpoints são suficientes para 90% dos casos

Com `useBreakpoints()` e `useWebMediaQueries()`, consegui layouts responsivos sem CSS-in-JS complexo ou bibliotecas pesadas.

### 6. Tema como parâmetro de função é elegante

O padrão `const s = (theme) => StyleSheet.create({...})` é simples, performático e permite temas dinâmicos.

### 7. Commits contam a história real

Os 150+ commits deste projeto documentam não apenas o que funcionou, mas também o que foi tentado e abandonado. Essa honestidade é valiosa para outros desenvolvedores.

---

## Referências

- **Ignite (Infinite Red):** [github.com/infinitered/ignite](https://github.com/infinitered/ignite)
- **Documentação Ignite:** [docs.infinite.red/ignite-cli](https://docs.infinite.red/ignite-cli/)
- **Bluesky Social App:** [github.com/bluesky-social/social-app](https://github.com/bluesky-social/social-app)
- **react-responsive:** [npmjs.com/package/react-responsive](https://www.npmjs.com/package/react-responsive)
- **Expo:** [expo.dev](https://expo.dev)

---

*Este artigo foi escrito com base na análise de 150+ commits do projeto Econolista, comparação direta com o código-fonte do Bluesky Social App, e experiência pessoal com o boilerplate Ignite.*
