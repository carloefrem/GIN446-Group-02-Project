# AI Development Log

AI tool used: Gemini

## Interaction 1: Weak prompt experiment (Section 5.1)

Task:
Test what happens when AI is asked to do the whole job, to compare it with the constrained prompts used later.

My prompt:
"Create a complete responsive CSS Grid website for me."

AI recommendation:
Gemini generated a full page with its own header ("CSS Grid Architecture"), its own menu items, and a dashboard with a sidebar and three side panels. When the starter code was provided, it kept the six cards and three columns but turned the whole body into a grid: the navigation became a vertical sidebar and the footer became a tall block.

My decision:
REJECT

Why:
The page ignored the lab requirements (my starter HTML, six cards, Flexbox navigation) and I could not easily explain code I did not write. This approach also goes against the lab rule that AI must not generate the complete solution.

Verification:
Opened both results in the browser and compared them with the starter HTML and the requirements. The first result did not contain my content; the second changed the structure of the page.

What I learned:
A weak prompt gives code that looks complete but may not meet the requirements and is hard to understand. Constrained prompts that ask for explanation or review are more useful.

## Interaction 2: Review of my CSS Grid code (Section 5.2)

Task:
Have the AI explain and review my manual Grid (display: grid, repeat(3, 1fr), gap) without rewriting my project.

My prompt:
I pasted my HTML and CSS and asked Gemini to explain what each important line does, to ask me three short questions to check my understanding, and not to rewrite my project.

AI recommendation:
Gemini explained display: grid, gap, fr, repeat() and minmax(), then asked three check questions (which element is the container, why the columns are equal, why gap is better than margins). It made three suggestions: (1) use repeat(3, 1fr) instead of percentages, (2) use repeat(auto-fit, minmax(min(100%, 280px), 1fr)) for the cards, (3) use grid-template-areas on the body with a mobile-first single column and a 200px sidebar from 768px.

My decision:
Suggestion 1: ACCEPT. Suggestions 2 and 3: MODIFY.

Why:
Suggestion 1 matched what I already had and avoids overflow because fr units are calculated after the gaps are removed. Suggestion 2 is fluid, but the lab needs exactly 3 / 2 / 1 columns, and inside the AI's sidebar layout the card area is about 480px at 768px, which gives 1 column instead of 2. I used explicit media queries (600px and 1024px). Suggestion 3 is readable, but the sidebar turns the Flexbox navigation into a vertical list and the 1fr row stretches the footer, so I kept my page structure and used Grid only for the cards.

Verification:
Tested in Chrome DevTools at 1440, 768 and 375 px. Result: 3, 2 and 1 columns with no horizontal scrollbar, the navigation stays a horizontal Flexbox row that wraps on mobile, and the footer height matches its text. I also compared 3 x 33.33% with repeat(3, 1fr) at gap: 20px and confirmed that percentages overflow.

What I learned:
The AI's explanations were useful, but a suggestion can fix one problem and break a requirement. gap creates space only between tracks, and 1fr means a share of the free space, not 100% of the screen. I have to check each suggestion against the assignment.

## Interaction 3: Tutor prompt (Section 5.3)

Task:
Understand why repeat(3, 1fr) gives three equal columns and how fr differs from percentages.

My prompt:
I asked Gemini to explain why repeat(3, 1fr) creates three equal columns and to compare fr units with percentages using a small example, without rewriting my project.

AI recommendation:
repeat(3, 1fr) is shorthand for 1fr 1fr 1fr. The browser subtracts the gaps, then divides the remaining space equally. With a 1000px container and a 20px gap, three 33.33% columns need 1040px and overflow by 40px, while three 1fr columns get 320px each and fit.

My decision:
ACCEPT

Why:
The explanation is correct and I verified the numbers myself: 3 x 333.3 + 2 x 20 = 1040px, and (1000 - 40) / 3 = 320px.

Verification:
Reproduced the example in the browser with a 1000px container and checked the column widths in DevTools.

What I learned:
Percentages are based on the container width and ignore the gap, so they can overflow. fr units work with the space that remains after the gaps, which is why I use them for the cards.
