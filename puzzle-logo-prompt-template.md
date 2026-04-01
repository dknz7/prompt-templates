# Flat Puzzle-Piece Tech Logo Prompt Template

When generating this specific aesthetic in the future, the key is to be extremely strict about **flat 2D vectors**, **puzzle-piece interlocking**, separating the **outer shape** from the **inner core**, and aggressively negative-prompting any 3D or "busy" elements.

Here is the agnostic prompt template you can use for any future shape (like a magnifying glass, a shield, an hourglass, etc.):

### The Prompt

```json
{
  "prompt": "A perfectly flat, minimalist 2D vector-style tech logo of an abstract geometric [INSERT MAIN SHAPE HERE, e.g., magnifying glass]. The design methodology uses a highly deliberate puzzle-piece layout composed of exactly 6 to 8 distinct, smooth, sweeping, curvilinear white geometric shapes. These white shapes are NOT blocky; they feature fluid curves that perfectly interlock and fit identically flush to each other, separated entirely by ultra-thin, razor-sharp, uniform negative-space gaps. CRITICAL DETAIL: The overall outer boundary MUST explicitly form a distinct, recognizable [INSERT MAIN SHAPE HERE] structure. It is purely 2D, with zero lighting, zero shadows, and zero 3D rendering elements. In the exact center, tightly surrounded by the flowing interlocking white pieces, is a singular, stylized [INSERT CORE ICON HERE, e.g., spark, eye, star, flame]. The center icon must be ONE SOLID, UNBROKEN SHAPE of vibrant flat [INSERT COLOR HERE, e.g., purple (#8B5CF6)]. It must NOT have any inner black shapes, cut-outs, outlines, or gradients. It is a single solid block of color. The curved white puzzle pieces must fit perfectly right up against the exact contours of the solid center icon. The background is a solid dark color (#08080c). The design must feel like a complex precision-engineered modern SaaS company logo—highly scalable, moderately fractured into 6-8 fluid pieces forming a literal [INSERT MAIN SHAPE HERE].",
  
  "negative_prompt": "blocky shapes, sharp right angles, hard squares, black inner [icon], black cut-out [icon], overlapping inner shapes, black hole in [icon], 3D render, photorealistic, photography, shading, lighting effects, specular highlights, drop shadows, gradients, bevels, embossing, wireframes, overlapping lines, swirling lines, literal lightbulb, gradient fills, text, letters, typography",
  
  "settings": {
    "style": "flat 2D minimalist corporate vector icon",
    "lighting": "unlit, completely flat fill, graphic design",
    "camera_angle": "straight-on 2D profile",
    "depth_of_field": "infinite, totally flat",
    "quality": "crisp vector style, absolute geometric perfection"
  }
}
```

### Why This Works:
1. **"6 to 8 distinct, smooth, sweeping, curvilinear white geometric shapes"**: This forces the AI to break the shape into a puzzle, but strictly limits the complexity so it doesn't look like a garden maze.
2. **"Fit identically flush... separated entirely by ultra-thin, razor-sharp, uniform negative-space gaps"**: This creates the locked-in glassmorphism/blueprint vibe without actual 3D shadows.
3. **"ONE SOLID, UNBROKEN SHAPE... NOT have any inner black shapes [or] cut-outs"**: Generative AI *loves* layering things. You have to aggressively yell at it to just paint one solid flat vector shape.
4. **The Negative Prompts**: Blocking "3D render", "drop shadows", and "gradients" is what keeps it looking like a professional, scalable SVG logo rather than a digital painting.
