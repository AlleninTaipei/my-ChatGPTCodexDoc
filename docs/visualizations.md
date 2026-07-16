# Visualizations

Turn ideas and information into interactive visual explanations in ChatGPT

## Overview

Visualizations convert questions, ideas, and information into charts, maps, diagrams, calculators, simulations, and interactive explainers within ChatGPT conversations. They're useful when adjusting inputs or understanding relationships would clarify answers, enable comparisons, support practice, or facilitate action.

## Availability

The Visualizations preview is rolling out with availability depending on your plan, platform, account, and workspace settings.

### Current availability by surface

| Surface | Status |
|---------|--------|
| ChatGPT on the web | Available to supported accounts in Chat and ChatGPT Work |
| ChatGPT desktop app | Rolling out in preview |
| ChatGPT mobile apps | Rolling out to eligible accounts; composer controls vary by app version |
| Codex CLI and IDE extension | Visualization rendering not supported |

## How to access

**Desktop app:** Type `@` in the composer, begin entering "Visualize", and select **Visualize** under **Plugins**. The composer adds a **Visualize** tag before your request.

**Web:** In supported Chat or ChatGPT Work conversations, type `@` in the composer, start entering "Visualize", and select **Visualize** under **Plugins** (description: "Create visualizations and interactive tools").

**Alternative:** Type `@Visualize` and select the matching suggestion.

**Note:** Codex CLI doesn't render Visualizations. Open content in ChatGPT on the web or desktop app, then tag `@Visualize`. The Codex IDE extension also doesn't support Visualization rendering.

## When to use visualizations

ChatGPT can choose visual formats when they materially improve answers. You can also tag `@Visualize` when specifically requesting interactive results.

Use the smallest format fitting the task:
- **Diagrams:** labeled relationships or processes
- **Charts/plots:** named numeric data and comparisons
- **Maps:** geographic information
- **Interactive visualizations:** when inputs, time, motion, or spatial relationships should change
- **Sites:** durable hosted applications with shareable URLs, permissions, or persistent data

## Effective prompting strategy

Strong requests identify the outcome, source material, question, and useful interactions. Include information source details (conversation content, pasted data, attached files, or connected sources) and consider higher reasoning settings for complex requests.

## Interactive examples

Three visualizations demonstrate focused prompts becoming interactive explanations:

### Spirograph
Create an interactive spirograph explaining rolling circle mechanics. Adjust radii, pen distance, and animation speed to observe how a rolling circle traces curves using the equation: `x=(R-r)cos(t)+d cos(((R-r)/r)t)   y=(R-r)sin(t)-d sin(((R-r)/r)t)`

### Wave interference
Interactive lab worksheet for adjusting wave sources and probing interference fields. Modify wavelength, source spacing, and phase offset while recording observations about nodal patterns and bright-band positions.

### Tokenizer explainer
Interactive demonstration of GPT tokenization process: "text → UTF-8 bytes → ranked byte-pair merges → integer token IDs". Edit text and step through tokenization stages with a teaching vocabulary (actual models use different vocabularies and IDs).

## Refining results

Continue in the same conversation describing desired changes:
- Add or remove controls, filters, comparisons, or annotations
- Correct source data, units, labels, or assumptions
- Simplify slow results by aggregating, binning, or sampling
- Add text summaries and data tables
- Ensure keyboard accessibility with visible focus states
- Use labels/patterns alongside color; remove looping motion
- Convert to Sites for hosting and persistent access

## Sharing and reuse

Use the conversation's standard **Share** action when available. Review the entire shared conversation first, including source data and earlier messages. Visualizations typically represent information snapshots when created, not live dashboards synchronized with connected sources.

Generated download controls and export formats vary by result. If exports fail, request underlying data in simpler formats or ask ChatGPT to convert to a Site.

## Accessibility considerations

Generated visualizations aim for semantic controls, visible focus, readable contrast, and reduced motion, though results vary. Before sharing:
- Check the visualization quality
- Request text summaries and data tables
- Label axes and units
- Avoid color-only distinctions
- Ensure keyboard control functionality

## Troubleshooting failed results

Visualizations may take a minute or longer to generate. If results are blank or missing:
1. Wait for response completion
2. Reload the conversation once
3. Retry the request

If issues persist:
- Request simpler visualizations
- Aggregate, bin, downsample, or reduce precision in large datasets
- Remove generated controls or libraries not functioning
- Verify important values, geographic boundaries, and source assumptions
- Request charts, diagrams, tables, or Sites instead

## Data handling

Apply the same data-handling judgment used in standard ChatGPT conversations. Include sensitive information only when organizationally permitted, and review full conversations before sharing.

---

**Related documentation:** Sites, Projects/chats/tasks, Work with files, Image generation
