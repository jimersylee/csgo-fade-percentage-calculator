# CS:GO Fade Percentage Calculator

![NPM Version](https://img.shields.io/npm/v/csgo-fade-percentage-calculator)
![NPM Bundle Size](https://img.shields.io/bundlephobia/min/csgo-fade-percentage-calculator?label=size)
![NPM Downloads](https://img.shields.io/npm/dm/csgo-fade-percentage-calculator)
![GitHub License](https://img.shields.io/github/license/chescos/csgo-fade-percentage-calculator)
![GitHub Test Workflow Status](https://img.shields.io/github/workflow/status/chescos/csgo-fade-percentage-calculator/Test/master?label=tests)
![GitHub Build Workflow Status](https://img.shields.io/github/workflow/status/chescos/csgo-fade-percentage-calculator/Build/master?label=build)

Calculate the Fade percentage value of a CS:GO skin based on a given seed value. Supporting all
[Fade skins](https://csgoskins.gg/families/fade) except for gloves. Easily convert every paint seed
(also called pattern index) into a fade percentage value, or get a full list of all paint seeds and
the corresponding fade percentages.

## 🚀 Installation

In order to install the latest package version from
[NPM](https://www.npmjs.com/package/csgo-fade-percentage-calculator), simply run:

```bash
npm install csgo-fade-percentage-calculator
```

## 🛠 Usage

```js
const { FadeCalculator } = require('csgo-fade-percentage-calculator');

// Get a list of all fade percentages for all available weapons.
const allFadePercentages = FadeCalculator.getAllFadePercentages();

// Get a list of fade percentages for the Karambit.
const karambitFadePercentages = FadeCalculator.getFadePercentages('Karambit');

// Get the fade percentage for the AWP and the seed 123.
const awpFadePercentage = FadeCalculator.getFadePercentage('AWP', 123);

// Get all supported weapons.
const supportedWeapons = FadeCalculator.getSupportedWeapons();
```

## 📜 How It Works

Each CS:GO weapon skin has a random paint seed value between 0 and 999. This paint seed value, sometimes also
called pattern index, determines the positioning of the pattern on the gun. Specifically, the paint seed determines
the X offset, Y offset, and rotation value for the pattern position. Those three values can be calculated using
an algorithm which has been open-sourced by Valve.

Luckily, all Fade skins have the same X and Y offsets, and only the rotation value changes with each paint seed.
This package simply converts paint seeds to rotation values, and then assigns each rotation value a fade percentage
between 80 and 100, where the lowest rotation value is a 100% Fade, and the highest rotation value is an 80% Fade.

The whole process just involves simple math, and it is superior to alternative methods such as image pixel color
analysis for various reasons:

- The resulting Fade percentage values are much more accurate
- The methodology can be easily verified and reproduced
- The algorithm is simple and fast

## 🌎 Platform Support

There are already a few platforms that use the open-source algorithm implemented by this package to generate
fade percentage values. The sites below all share the same fade percentage values, which can also be generated by
this package:

- [Skinport](https://skinport.com/)
- [CSGOSKINS.GG](https://csgoskins.gg/)
- [SkinsMonkey](https://skinsmonkey.com/)

Other sites are currently known to use their own algorithms, probably based on image analysis. These sites come
to different conclusions which paint seed corresponds to which fade value, as pixel color analysis is not
very accurate to determine a fade value:

- [CS.MONEY](https://cs.money/)
- [BUFF163](https://buff.163.com/)
- [BUFF Market](https://buff.market/)
- [SkinBid](https://skinbid.com/)
- [BroSkins](https://broskins.com/)

## 💻 Other Programming Languages

You're not using Node, JavaScript, or TypeScript for your project? We have a
[pre-generated JSON file](https://raw.githubusercontent.com/chescos/csgo-fade-percentage-calculator/master/generared/fade-percentages.json)
for you that contains the fade percentages of all supported weapons. You can download it with any programming language
into your project to store and process the values there.

Alternatively, feel free to check out the source code of this library and port it into your preferred language.

## ❓ Frequently Asked Questions

### How accurate are the generated fade percentages?

The generated fade percentage values are basically as accurate as they can get. Fade percentages don't really exist
in CS:GO itself, it's a community driven value which has been historically computed through image analysis of
screenshots. However, this method is not very accurate as it depends on many presumptions of the used algorithm.
This library just reverses the algorithm which Valve uses internally to apply the pattern on the weapon, and our only
presumption is a fade percentage range between 80% and 100%, with 80% being the worst and 100% being the best value.

### Why do some websites display different fade percentages?

Some websites may use different algorithms to calculate fade percentages, often based on pixel color analysis of
screenshot images. Those algorithms can come to very different conclusions about what seed value corresponds to which
fade percentage. Hopefully, the community will shift from image analysis to the open-source algorithm used by this
library, creating a stronger consensus.

### Why are gloves not supported?

Glove skins are handled differently in CS:GO. The method used by this library to compute fade values does not work
for glove skins, because their patterns shift more unpredictably, similar to Case Hardened skins.

## ⭐ Credits

A big thanks goes to [Step7750](https://github.com/Step7750), the work on this library was inspired by
[his research](https://www.reddit.com/r/GlobalOffensiveTrade/comments/b7g538/psa_how_paint_seed_actually_works_technical/)
about CS:GO paint seeds. He also implemented Valve's uniform random number generator
[in Go](https://github.com/Step7750/UniformRandom), which was ported to TypeScript for this project.
