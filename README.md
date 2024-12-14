# Gemini-2.0
https://github.com/google-gemini/starter-applets.git
git clone https://github.com/Hirendra-creater/Gemini - 2.0.git
https://github.com/Hirendra-creater/bio.site/hmitra.git
https://github.com/google-gemini/starter-applets?tab=readme-ov-file#ai-studio-starter-applets
google-gemini/starter-applets


// Copyright 2024 Google LLC

// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at

//     https://www.apache.org/licenses/LICENSE-2.0

// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

// script.js
// Authors: kylephillips@ bencobley@

import * as aistudio from "./aistudio.js";
import * as mapsFunction from "./function-declarations.js";
import { presets } from "./presets.js";
import {
  html,
  render,
} from "https://cdn.jsdelivr.net/gh/lit/dist@3/core/lit-core.min.js";

const systemInstructions = mapsFunction.systemInstructions;

const functionDeclarations = mapsFunction.declarations.map(declaration => ({
  ...declaration,
  callback: (args) => {
    const { location, caption } = args;
    renderPage(location, caption);
  },
}));

let chat;

async function init() {
  renderPage("%"); // Start by rendering with empty location query: shows earth
  try {
    const initResult = await aistudio.init({
      systemInstructions,
      functionDeclarations,
    });

    // Handle light or dark theme from parent
    if (initResult.theme === "light") {
      document.documentElement.setAttribute("data-theme", "light");
    } else if (initResult.theme === "system") {
      if (
        window.matchMedia &&
        window.matchMedia("(prefers-color-scheme: dark)").matches
      ) {
        document.documentElement.removeAttribute("data-theme"); // Use default (dark)
      } else {
        document.documentElement.setAttribute("data-theme", "light");
      }
    }

    aistudio.chat("Welcome to Map Explorer!");

    chat = async (userText) => {
      try {
        const temperature = 2; // High temperature for answer variety
        const response = await aistudio.generateContent({ userText, temperature });
        console.log(response);
      } catch (e) {
        console.error(e);
      }
    };
  } catch (error) {
    console.error("Error initializing AI Studio:", error);
  }
}

init();

function renderPage(location, caption = "") {
  const root = document.querySelector("#root");
  caption = caption.replace(/\\/g, '');
  render(
    html`
      <div id="map">${mapsFunction.embed(location)}</div>
      ${caption
        ? html`<div id="caption"><p>${caption}</p></div>`
        : ""}
      <div id="presets-container">
      <span id="take-me-somewhere">Take me somewhere...</span>
        <div id="presets">
              ${presets.map(
          ([name, message]) =>
            html`<button @click=${() => chat(message)} class="preset">
                    ${name}
                  </button>`
        )}
        </div>
      </div>
    `,
    root
  );
}

// Copyright 2024 Google LLC

// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at

//     https://www.apache.org/licenses/LICENSE-2.0

// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  base: './',
  plugins: [react()],
})

@import './player.sass'
@import './chart.sass'

$baseFont: 'Space Mono', monospace

.light
  --background: #F3F3F6
  --text: #1A1C1E
  --border: #C6C6C9
  --highlight: #fff
  --track: #86878A
  --link: #2872E3

.dark
  --background: #1A1C1E
  --text: #fff
  --border: #37393C
  --highlight: #000
  --track: #37393C
  --link: #87A9FF

*
  margin: 0
  padding: 0
  box-sizing: border-box
  font-variant-ligatures: none

:root
  --track-fill: #fff
  --mid: #757575
  font-family: $baseFont

#root
  width: 100vw
  height: 100vh

h1, h2, h3, h4, h5, h6
  font-weight: normal

li
  list-style: none

input, textarea
  font-family: $baseFont
  background: none
  color: var(--text)
  border: none
  outline: none
  font-size: 14px
  resize: none
  user-select: text

  &::placeholder
    user-select: none

textarea
  width: 100%
  background: var(--highlight)
  border: 1px solid var(--border)
  border-radius: 8px
  font-size: 14px
  padding: 10px 15px
  margin-bottom: 10px

  &.active
    border-color: var(--text)

[role='button']
  cursor: pointer

button
  font-family: $baseFont
  background: none
  color: var(--text)
  border: none
  font-size: 14px
  cursor: pointer
  user-select: none
  display: flex
  align-items: center
  gap: 5px

  &:focus
    outline: none

  &[disabled]
    opacity: .5
    cursor: not-allowed

  .icon
    display: block

.button
  padding: 8px 10px
  border-radius: 6px
  display: flex
  align-items: center
  justify-content: center
  gap: 5px

  &.inactive
    opacity: .3
    pointer-events: none

.icon
  font-family: 'Material Symbols Outlined'
  font-weight: 300
  line-height: 1

main
  max-width: 1200px
  height: 100vh
  display: flex
  flex-direction: column
  margin: 0 auto
  overflow: hidden
  background: var(--background)
  color: var(--text)

.top
  display: flex
  min-height: 50vh
  border-bottom: 1px solid var(--border)

.tools
  display: flex
  gap: 20px
  flex: 1
  overflow: hidden
  transition: opacity .2s

  &.inactive
    opacity: .2
    pointer-events: none

.collapseButton
  border-left: 1px solid var(--border)
  width: 20px
  display: flex
  align-items: center
  justify-content: center

  .icon
    font-size: 24px
    color: var(--text)

.modeSelector
  display: flex
  flex-direction: column
  gap: 30px
  overflow: hidden
  width: 250px

  &.hide
    width: 0

  > div
    padding: 20px 15px

    &:first-child
      flex: 1
      overflow: auto

    &:last-child
      border-top: 1px solid var(--border)

  h2
    font-size: 14px
    color: var(--text)
    white-space: nowrap
    margin-bottom: 15px

  &.inactive
    opacity: .2
    pointer-events: none

.modeList
  flex-direction: column
  display: flex
  gap: 10px

  .button
    justify-content: flex-start
    gap: 12px
    transition: background .2s
    background: none
    outline: 1px solid var(--border)
    white-space: nowrap
    border-radius: 8px
    min-width: fit-content
    width: 100%

    &:hover
      background: var(--border)

    &.active
      outline: 2px solid var(--text)

  + textarea
    margin-top: 10px

.generateButton
  padding: 12px 20px
  background: var(--highlight)
  width: 100%
  border: 1px solid var(--border)

.backButton
  border-top: none !important

  button
    font-size: 14px

.output
  flex: 1
  padding: 20px 15px
  overflow: auto
  position: relative

  &:hover
    .sentence
      opacity: .5

  time
    color: var(--link)
    padding: 2px 5px
    border-radius: 4px
    font-size: inherit
    text-decoration: underline

  .sentence
    font-size: 18px
    line-height: 1.8
    display: inline
    cursor: pointer
    transition: opacity .2s

    &:hover
      opacity: 1

    time
      margin-right: 8px

  ul
    display: flex
    flex-direction: column
    gap: 5px

    button
      font-size: 15px
      display: flex
      gap: 15px
      text-align: left
      padding: 10px 15px
      border-radius: 6px
      width: 100%

      &:hover
        background: var(--border)

      p
        font-size: 14px

.modeEmojis
  .sentence
    font-size: 40px
    margin-right: 20px

    time
      top: -8px

.modeHaiku
  .sentence
    display: block
    font-size: 20px

    time
      top: -5px

.modeTable
  table
    width: 100%
    border-collapse: collapse

  th
    text-align: left

  th, td
    padding: 10px

  tr
    border-bottom: 1px solid var(--border)
    display: table-row

  thead tr:hover
    background: transparent

.modelSelector
  position: relative
  margin-bottom: 10px
  position: relative

  &::after
    content: '▾'
    display: block
    position: absolute
    right: 10px
    top: 50%
    transform: translateY(-55%)

  select
    width: 100%
    appearance: none
    color: var(--text)
    background: var(--border)
    border: 1px solid var(--border)
    border-radius: 6px
    padding: 5px 10px
    font-family: $baseFont
    outline: none

.loading
  span
    display: inline-block
    animation: loading steps(4, jump-none) 777ms infinite
    width: 0
    overflow: hidden
    vertical-align: bottom

@keyframes loading
  to
    width: 30px
