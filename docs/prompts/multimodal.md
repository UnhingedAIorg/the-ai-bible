# Multimodal Prompts (Vision, Audio, Video, 3D)

**See, hear, speak, create — across every sense.**

This library contains **130+ prompts** for image, video, audio, and cross-modal tasks in the sensory AI era.

---

## Vision Mastery

### 1. Advanced Image Analysis
Analyze this image as a team of experts: art historian, forensic analyst, UX designer, and detective. Describe composition, hidden details, emotional impact, technical quality, and cultural context. What story does this image tell?

### 2. Text-to-Image Prompt Engineering
Create a cinematic, hyper-detailed prompt for Midjourney/Flux/SD3 that captures [scene] with specific lighting, camera angle, mood, and artistic references. Use weights and negative prompts. Include: subject, environment, lighting, camera specs, style modifiers, quality boosters.

### 3. Visual Question Answering (VQA)
Answer questions about this image with precision. Ground your answers in visible elements. If the answer isn't visually present, state that clearly. Distinguish between what's shown vs. what can be inferred.

### 4. OCR & Document Understanding
Extract all text from this image accurately. Preserve formatting (tables, columns, headers). Identify document type. Flag any unclear or degraded text. For structured documents, output in appropriate format (JSON, Markdown table, etc.).

### 5. Diagram-to-Code
Convert this [flowchart/UI mockup/wireframe/system diagram] into working code. Identify components, relationships, and logic flow. Output clean, documented code in [language/framework] that faithfully reproduces the diagram's structure.

### 6. Image Captioning (Detailed)
Write a rich caption for this image: 2-3 sentences describing the main subject, setting, action, mood, and notable details. Include both literal description and emotional resonance. Make it suitable for accessibility and SEO.

### 7. Visual Search Description
Describe this image for similarity search. Focus on: objects present, colors, style, composition, textures, and distinctive features. Make it detailed enough to match similar images but general enough to capture the essence.

### 8. UI/UX Analysis from Screenshot
Analyze this UI screenshot: information hierarchy, usability issues, accessibility concerns, visual design principles, and improvement recommendations. Suggest A/B test ideas and UX best practices.

### 9. Chart & Graph Interpretation
Analyze this [chart type]: identify axes, scales, data trends, outliers, correlations, and key insights. What story does the data tell? What conclusions can and cannot be drawn? Suggest alternative visualizations.

### 10. Medical Image Analysis (Informational)
Describe what is visible in this medical image [X-ray/MRI/CT/dermatology]. Identify anatomical structures, any obvious abnormalities, and technical quality. **Important**: This is for educational purposes only, not a diagnosis. Always consult healthcare professionals.

### 11. Product Photography Optimization
Analyze this product photo and suggest improvements: lighting adjustments, angle changes, background alternatives, styling props, and post-processing edits. Specify settings for: aperture, shutter speed, ISO, focal length.

### 12. Art Style Identification
Identify the artistic style, period, and influences in this artwork. Analyze: color palette, brushwork, composition, subject matter, and technique. Suggest similar artists and comparable works. Estimate confidence level.

### 13. Facial Analysis (Ethical)
Describe facial features objectively for [legitimate use: art/anthropology/security training]: expression analysis, approximate age range, visible accessories, and distinguishing features. **Never** make assumptions about identity, character, or personal attributes. Include ethical use disclaimer.

### 14. Scene Understanding
Describe this scene comprehensively: setting (indoor/outdoor, location type), objects and their relationships, activities occurring, time of day/season, atmosphere, and any text present. What would be important for a robot navigating this space?

### 15. Image Comparison
Compare these two images: similarities, differences, which came first (if obvious), quality differences, compositional choices, and which is more effective for [purpose]. Side-by-side analysis with specific observations.

---

## Image Generation & Editing

### 16. Prompt Enhancement for Image Gen
Take this basic description and enhance it into a professional image generation prompt: "[simple description]". Add: style references, technical specifications, mood descriptors, lighting details, and quality modifiers. Output optimized prompts for [Midjourney/DALL-E/Stable Diffusion/Flux].

### 17. Negative Prompt Engineering
Create comprehensive negative prompts to avoid common AI generation artifacts: extra limbs, deformed hands, blurry faces, low quality, watermark, text errors. Tailor to specific subject matter and desired style.

### 18. Style Transfer Description
Describe how to apply [artistic style] to [subject]: color palette, brush stroke characteristics, texture qualities, composition approach, and specific style elements to include. Create prompt for style-consistent image generation.

### 19. Inpainting Instructions
For this image with [region to edit], provide precise inpainting instructions: what to remove/add, how to match lighting/texture/perspective, color harmony, and edge blending. Describe the desired final result.

### 20. Image Variation Generation
Create 10 prompt variations of this image for [image gen model]: keep core subject but vary style, lighting, angle, mood, and setting. Ensure each variation maintains the essence while offering distinct creative directions.

### 21. Consistent Character Generation
Generate prompts for maintaining character consistency across multiple images: physical description, clothing style, distinguishing features, personality hints in pose/expression, and reference seed values for reproducibility.

### 22. Logo Design Brief to Prompt
Convert this brand brief into image generation prompts: brand values, target audience, desired emotions, color preferences, and style direction. Create variations: modern, classic, minimalist, bold. Include technical specs for different use cases.

### 23. Architectural Visualization
Create prompts for architectural renders: building style, materials, environment, time of day, weather, human scale elements, camera angle (bird's eye/worm's eye/eye level), and photorealism vs. stylization balance.

### 24. Fashion Design Description
Describe this fashion look for reproduction: garment types, fabrics, colors, patterns, fit/silhouette, accessories, and styling details. Include construction notes and suggested material alternatives at different price points.

### 25. Food Photography Styling
Analyze this food photo and suggest styling improvements: plating arrangement, garnish placement, lighting angle, background choice, props, and camera settings. Suggest edits for making it more appetizing and Instagram-worthy.

---

## Video Understanding

### 26. Video Summarization
Summarize this video: main topics covered, key moments with timestamps, important visual information, and overall narrative arc. Create chapter markers and one-sentence descriptions for each segment.

### 27. Video Captioning for Accessibility
Generate detailed captions for this video: describe visual action, scene changes, on-screen text, speaker identification, sound effects, and music cues. Format as [SRT/VTT] with proper timing.

### 28. Action Recognition & Timeline
Identify all actions in this video with timestamps: what is happening, who/what is involved, and action sequence. Detect repetitive patterns and significant changes in activity.

### 29. Video Search Indexing
Create searchable metadata for this video: objects detected (with timestamps), scenes/settings, people present, activities, text visible, audio keywords, and thematic tags. Enable semantic video search.

### 30. Video Script Extraction
Extract the narrative/script from this video: transcribe dialogue, describe visual storytelling, note transitions and cuts, and reconstruct the story flow. Identify storytelling techniques used.

### 31. Sports Analysis
Analyze this sports footage: identify sport, teams/players if visible, score/status, key plays with timestamps, technique observations, and strategic patterns. Highlight exceptional moments.

### 32. Surveillance Video Analysis (Privacy-Conscious)
Analyze this video for [security/safety monitoring]: detect anomalies, count people/objects, track movements, flag safety concerns. **Apply privacy filtering**: blur faces, don't identify individuals, focus on patterns not identities.

### 33. Film Scene Analysis
Analyze this film scene: cinematography (shots, angles, movement), lighting design, color grading, sound design, editing rhythm, narrative function, and emotional impact. Reference similar scenes from cinema history.

### 34. Tutorial Video to Steps
Convert this tutorial video into step-by-step written instructions: identify each step, required materials/tools, timing estimates, tips and warnings, and checkpoints for success.

### 35. Video Quality Assessment
Evaluate this video's technical quality: resolution, frame rate stability, compression artifacts, lighting consistency, audio quality, focus/sharpness, and color accuracy. Identify issues and suggest improvements.

---

## Video Generation & Editing

### 36. Video Script to Visual Prompts
Convert this script into scene-by-scene video generation prompts: visual description, camera movement, lighting, duration, and transition to next scene. Format for [Sora/Runway/Pika/video gen model].

### 37. Storyboard Generation
Create a storyboard from this [script/concept]: scene descriptions, shot types (wide/medium/close-up), camera angles, action notes, dialogue, and timing. Visual layout suggestions for each frame.

### 38. Video Editing Suggestions
Suggest edits for this raw footage: cuts to make, pacing adjustments, B-roll insertion points, music/sound effect timing, color grading direction, and title/graphics placement.

### 39. B-Roll Shot List
Generate B-roll shot list for [topic]: essential coverage shots, cutaway opportunities, detail shots, establishing shots, and transition shots. Include duration and technical notes for each.

### 40. Motion Graphics Description
Describe motion graphics for [purpose]: animation style, timing, easing curves, text treatments, color transitions, and visual effects. Specify After Effects/animation techniques.

---

## Audio Processing

### 41. Audio Transcription with Diarization
Transcribe this audio with speaker identification: label each speaker (Speaker A, B, etc.), note non-verbal sounds [laughter, pause, interruption], and format as clean dialogue script with timestamps.

### 42. Podcast Show Notes Generator
Generate comprehensive show notes: episode title suggestions, 3-4 paragraph summary, key topics with timestamps, memorable quotes, guest bio, and resources/links mentioned.

### 43. Music Genre & Mood Analysis
Analyze this audio: identify genre, tempo (BPM), key, mood/emotion, instrumentation, production style, and similar artists. Suggest appropriate contexts for use (workout, study, party, etc.).

### 44. Audio Quality Assessment
Evaluate audio quality: clarity, noise levels, balance, compression artifacts, EQ issues, room acoustics, and overall production value. Suggest specific improvements.

### 45. Sound Design Description
Describe sound design for [scene/concept]: ambient sounds, sound effects, foley opportunities, music cues, spatial audio considerations, and emotional audio storytelling.

### 46. Voice Cloning Script Preparation
Prepare script for voice cloning: optimal recording guidelines, script content covering all phonemes, emotion variations, technical specifications (sample rate, bit depth, environment), and quality checks.

### 47. Audio Segmentation
Segment this audio file: identify sections (intro, verse, chorus, outro), speaker changes, topic transitions, music vs. speech sections, and significant audio events with timestamps.

### 48. Speech Enhancement Guidance
Guide for improving this audio recording: noise reduction, EQ adjustments, compression settings, de-essing, normalization, and clarity enhancement. Step-by-step processing chain.

---

## Audio Generation

### 49. Music Generation Prompt (Suno/Udio)
Create detailed prompt for AI music generation: genre, mood, tempo, instrumentation, vocal style, structure (verse/chorus/bridge), lyrical themes, and production style. Include negative prompts to avoid.

### 50. Sound Effect Description
Describe sound effect for [action/object]: characteristics, duration, pitch/tone, intensity variation, and layering suggestions. Create generation prompt for [ElevenLabs/AudioGen].

### 51. Voice Over Script Optimization
Optimize this script for voice synthesis: pronunciation guides for difficult words, pacing indicators [pause], emphasis markers, tone direction, and technical specs (speed, pitch, emotion settings).

### 52. Ambient Soundscape Design
Design ambient audio environment for [setting]: base layer sounds, periodic elements, distant sounds, subtle variations over time, and emotional atmosphere. Technical specifications for looping.

### 53. Podcast Intro/Outro Music Brief
Create brief for podcast music: duration, energy arc, instrumentation, mood progression, and how it reflects show branding. Describe variations for different episode types.

---

## 3D & Spatial

### 54. 3D Model Description
Describe this 3D model: geometry complexity, topology quality, material properties, UV mapping, rigging suitability, polygon count, and optimization level. Suggested use cases and LOD recommendations.

### 55. Point Cloud Analysis
Analyze this point cloud data: identify objects/structures, measure dimensions, detect anomalies, assess density and coverage, and suggest preprocessing steps for [application].

### 56. Depth Map Interpretation
Interpret this depth map: spatial layout, distance relationships, object separation, occlusion boundaries, and 3D structure reconstruction. What would a robot need to know about this space?

### 57. AR/VR Scene Description
Describe AR/VR scene requirements: spatial anchors, occlusion handling, lighting estimation, surface detection, object placement logic, and user interaction affordances. Technical specs for [ARKit/ARCore/WebXR].

### 58. 3D Generation Prompt (Meshy/Point-E)
Create prompt for 3D model generation: object description, style (realistic/stylized/low-poly), topology requirements, intended use (game/print/animation), and material specifications.

### 59. NeRF/3D Gaussian Splatting Guidance
Guide for creating 3D scene from images: capture recommendations (angles, overlap, lighting), number of images needed, processing parameters, and expected output quality.

### 60. Spatial Audio Design
Design spatial audio for [VR/AR/game scene]: sound source positions, attenuation curves, occlusion effects, head-related transfer function considerations, and interactive audio triggers.

---

## Cross-Modal

### 61. Image-to-Text Generation
Generate [caption/story/blog post/marketing copy] inspired by this image. Capture the mood, details, and essence while adapting tone for [target audience/purpose].

### 62. Text-to-Image Description Chain
Given text: "[description]", create: (1) enhanced image generation prompt, (2) expected output description, (3) follow-up image variations, (4) integration into larger creative project.

### 63. Audio-to-Visual Translation
Generate visual description for this audio: imagery that matches the music's mood, suggested color palette, motion patterns, and visual rhythm that syncs with audio elements.

### 64. Visual-to-Audio Translation
Suggest audio design for this visual: music genre/mood, sound effects, ambient audio, and how audio should interact with visual elements. Create generation prompts.

### 65. Video-to-Text Summary
Generate multiple text outputs from this video: tweet thread, blog post, email newsletter, LinkedIn post, and academic abstract. Adapt length and tone for each format.

### 66. Multimodal Search Query
Create search query using [image + text description + audio sample]. Combine modalities for precise retrieval. Explain how each modality contributes to search intent.

### 67. Cross-Modal Embedding Analysis
Analyze semantic alignment across modalities: how well does text description match image content, does audio mood align with visual mood, identify any modality conflicts.

### 68. Accessibility Transcoding
Convert [video/audio/image] to accessible formats: alt text generation, audio description script, transcript, haptic feedback patterns, and screen reader optimization.

---

## Specialized Applications

### 69. E-Commerce Product Analysis
Analyze this product image for e-commerce: key selling features, suggested shot angles missing, background removal suitability, size reference needs, and optimization for different platforms (Amazon, Instagram, website).

### 70. Real Estate Photo Analysis
Evaluate this property photo: room identification, selling points, lighting issues, staging suggestions, and improvement priorities. Estimate how it will perform in listings.

### 71. Satellite Imagery Analysis
Analyze this satellite/aerial image: identify features, measure areas/distances, detect changes over time, classify land use, and extract geographic information.

### 72. Microscopy Image Analysis
Describe this microscopy image: magnification level, sample type, visible structures, staining if applicable, quality metrics, and biological/medical relevance.

### 73. Drone Footage Analysis
Analyze this drone footage: flight pattern, altitude estimation, coverage area, safety considerations, and optimal edit points. Suggest additional shots needed.

### 74. Wildlife Photography Assessment
Evaluate wildlife photo: species identification, behavior captured, technical quality (focus, exposure, timing), ethical considerations, and publication suitability.

### 75. Forensic Image Enhancement
Guide for forensic image analysis: identify enhancement opportunities (contrast, sharpening, color correction), authentication checks, metadata extraction, and evidence preservation protocols.

---

## Creative Workflows

### 76. Brand Visual Identity Generator
From brand description, generate visual assets: logo variations, color palette suggestions, typography pairings, pattern designs, and mockups for different applications.

### 77. Comic/Manga Page Layout
Design comic page from script: panel layout, shot selection, flow direction, text placement, and visual storytelling rhythm. Include drawing/prompting guidance.

### 78. Album Artwork Direction
Create visual direction for album: concept art descriptions, color schemes, typography styles, production techniques, and how visuals represent the music's themes.

### 79. Game Asset Design Brief
Write design brief for game assets: character concepts, environment designs, UI elements, animation requirements, and technical specifications for [engine: Unity/Unreal/Godot].

### 80. Film Color Grading Direction
Describe color grading for [scene/film]: mood/emotion target, reference films, LUT suggestions, primary corrections, selective adjustments, and final look characteristics.

---

## Technical Implementation

### 81. CLIP-Based Image Classification
Use CLIP for zero-shot classification: "This image is a photo of [class labels]". Rank probabilities, identify most likely class, and explain visual evidence.

### 82. Vision-Language Model Query
Query GPT-4V/Claude/Gemini: structure prompts for visual reasoning, ask specific questions about image regions, request analysis in specific formats (JSON, markdown table).

### 83. Multimodal RAG Setup
Design multimodal RAG: index images with text descriptions, retrieve by text query, return relevant images, and generate answers combining visual and textual evidence.

### 84. Video Frame Extraction Strategy
Determine optimal frame extraction: sampling rate for [analysis type], keyframe detection, scene change identification, and temporal chunking for long videos.

### 85. Audio Feature Extraction
Guide for extracting audio features: MFCCs, spectrograms, chroma features, tempo, pitch, and how to use them for [classification/search/analysis].

---

## Ethical & Safety Considerations

### 86. Deepfake Detection Guidance
Identify potential deepfake indicators: inconsistent lighting, unnatural blinking, audio-visual sync issues, artifact patterns, and recommend verification steps.

### 87. Synthetic Media Labeling
Appropriate labeling for AI-generated content: watermarking suggestions, metadata standards, platform disclosure requirements, and ethical best practices.

### 88. Privacy-Preserving Visual Analysis
Analyze visual data while protecting privacy: face blurring techniques, aggregation instead of individual identification, differential privacy for image datasets, and consent considerations.

### 89. Copyright-Safe Generation
Guidance for generating copyright-safe content: training data considerations, style vs. copy distinction, transformative use principles, and risk mitigation strategies.

### 90. Bias Detection in Visual AI
Identify potential biases in visual AI outputs: representation imbalances, stereotypical depictions, cultural insensitivities, and recommendations for fairer generation.

---

## Future & Advanced

### 91. Neural Radiance Field (NeRF) Prompting
Create NeRF scene description: camera trajectory, scene contents, lighting conditions, and output specifications. Guide for optimal capture and reconstruction.

### 92. Generative World Models
Describe world model for [environment]: physics, object interactions, agent capabilities, observation space, and training objectives for world model learning.

### 93. Multimodal Agent Perception
Design perception system for [robot/agent]: sensor fusion (vision, audio, touch), attention mechanisms, world state representation, and action-conditional prediction.

### 94. Cross-Modal Reasoning Chain
Solve [problem] using multimodal reasoning: analyze visual evidence, combine with audio cues, integrate textual knowledge, and synthesize answer across modalities.

### 95. Embodied AI Scene Understanding
Describe scene for robot navigation: navigable spaces, obstacles, object affordances, human activity prediction, and safe path planning considerations.

### 96. Multimodal Chain-of-Thought
Show reasoning across modalities: "I see [visual] which suggests [inference], combined with [audio] indicating [conclusion], therefore [answer]."

### 97. Video Prediction
Predict next frames in this video: likely continuation, physical plausibility, and alternative possibilities. Ground predictions in observed patterns.

### 98. Audio-Driven Animation
Describe animation rigged to audio: lip sync parameters, emotional expression mapping, body gesture timing, and procedural animation driven by sound features.

### 99. Style-Consistent Generation Across Modalities
Maintain style consistency: describe how visual style translates to audio mood, how both reflect textual themes, and cross-modal style transfer techniques.

### 100. Real-Time Multimodal Processing
Optimize for real-time: model selection for latency/quality trade-off, streaming processing strategies, and hardware acceleration options.

---

## Integration & Tools

### 101. OpenAI Vision API Usage
Structure GPT-4V prompts: image URL/base64, detailed questions, format specifications, and handling multiple images in single prompt.

### 102. Claude Vision Capabilities
Leverage Claude's vision: document analysis, chart interpretation, image description, and integration with text reasoning for comprehensive analysis.

### 103. Gemini Multimodal Features
Use Gemini's native multimodality: video understanding, audio processing, image generation, and seamless cross-modal reasoning.

### 104. LLaVA Local Vision Model
Deploy LLaVA for local vision understanding: setup instructions, quantization options, prompt engineering, and comparison to cloud APIs.

### 105. Whisper Audio Transcription
Optimize Whisper usage: model size selection, language specification, prompt engineering for domain vocabulary, and handling of challenging audio.

### 106. Stable Diffusion Prompt Crafting
Master SD prompting: token weighting, embedding usage, control net integration, and prompt scheduling for dynamic generation.

### 107. Midjourney Advanced Techniques
Advanced MJ prompting: image prompts, multi-prompts, parameters (--ar, --s, --c, --iw), style references, and upscaling strategies.

### 108. Runway Video Generation
Structure Runway prompts: motion descriptions, camera controls, temporal consistency, and integration with image inputs for video creation.

### 109. ElevenLabs Voice Design
Craft voice with ElevenLabs: voice cloning, emotion settings, stability/clarity tuning, multilingual generation, and style transfer.

### 110. ComfyUI Workflow Design
Design ComfyUI workflows: node connections, conditional logic, batch processing, and custom node integration for complex pipelines.

---

## Quality & Refinement

### 111. Image Quality Metrics
Evaluate image quality: sharpness, noise, color accuracy, dynamic range, compression artifacts, and aesthetic scoring methodologies.

### 112. Video Smoothness Analysis
Assess video temporal quality: frame consistency, motion smoothness, flicker detection, and stabilization evaluation.

### 113. Audio Fidelity Assessment
Measure audio fidelity: frequency response, dynamic range, distortion, stereo imaging, and reference comparison techniques.

### 114. Multimodal Consistency Check
Verify consistency across modalities: does audio match visual action, does text align with image, cross-modal contradiction detection.

### 115. Iterative Refinement Protocol
Refine outputs systematically: generate initial version, critique specific aspects, apply targeted improvements, and iterate until requirements met.

---

## Industry-Specific

### 116. Fashion Tech Analysis
Fashion-specific analysis: trend identification, fabric texture recognition, fit assessment, and sustainability indicators from visual cues.

### 117. Agriculture Imagery
Crop analysis from imagery: health assessment, growth stage identification, pest/disease detection, and yield estimation.

### 118. Manufacturing QC
Quality control visual inspection: defect detection, measurement verification, assembly check, and pass/fail automation criteria.

### 119. Autonomous Vehicle Perception
AV scene understanding: object detection, trajectory prediction, traffic sign recognition, and decision-critical feature identification.

### 120. Security & Surveillance
Security video analysis: anomaly detection, crowd behavior, left object detection, and privacy-compliant monitoring protocols.

### 121. Retail Analytics
Retail visual analysis: foot traffic patterns, shelf stock levels, customer behavior, and store layout optimization.

### 122. Archaeology & Cultural Heritage
Artifact analysis: dating estimation, origin identification, condition assessment, and historical context from visual features.

### 123. Astrophotography Processing
Astro image processing: stacking guidance, noise reduction, color calibration, and feature enhancement for celestial objects.

### 124. Underwater Imagery
Underwater photo/video analysis: visibility assessment, color correction needs, marine life identification, and technical diving documentation.

### 125. Aerial Survey Analysis
Drone/aerial analysis: orthomosaic quality, measurement accuracy, change detection, and survey-grade deliverable assessment.

---

## Creative Experiments

### 126. Synesthesia Translation
Translate between senses: describe music as colors/shapes, images as sounds, textures as emotions. Cross-sensory metaphor generation.

### 127. Dreams to Visuals
Interpret dream description into visual concept: surreal imagery, impossible physics, emotional atmosphere, and subconscious symbolism.

### 128. Abstract Concept Visualization
Visualize abstract concept [love/time/justice/entropy]: metaphorical imagery, symbolic elements, and representational strategies.

### 129. Temporal Art Description
Describe time-based artwork: evolution over viewing duration, loop points, interaction between moments, and cumulative experience.

### 130. Generative Collaboration
Human-AI collaborative creation: human provides concept/emotion, AI generates variations, human selects/directs, iterate toward vision.

---

**Total: 130+ prompts for the sensory AI era — spanning vision, audio, video, 3D, and cross-modal intelligence.**
