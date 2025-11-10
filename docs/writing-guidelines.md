# Writing Guidelines

## Words and Phrases to Avoid

Below is a list of words and phrases that should be avoided in blog posts. When writing new content, review this list to ensure clearer, more effective communication.

### AI-Generated Language Patterns
These phrases are commonly used in AI-generated content and should be avoided:
- "in this article, we will"
- "in this article, we'll"
- "the ever-changing * of"
- "it is important to note that"
- "in conclusion"
- "in summary"
- "a * tapestry of"
- "a tapestry of"
- "rich * tapestry"
- "the * realm of"
- "in the * world of"
- "a * testament to"
- "has emerged as a"
- "embark on a journey"
- "embark on a * journey"
- "embark on an * of"
- "embark on a * of"
- "navigate the uncharted"
- "a treasure trove of"
- "join us as we"
- "join us on this"
- "our * will provide"
- "a * exploration"
- "an * exploration"
- "beyond the surface allure"
- "this * invites you"
- "this * serves as your"
- "this * will serve as your"
- "traverse the diverse"
- "our * promises"
- "the annals of history"
- "in the * annals of"
- "the refined artistry"
- "cornerstone upon which"
- "in a * marked by"
- "the mysteries of"
- "this * aims to be your"
- "navigating a * maze of"
- "in the * landscape of"
- "the art of"
- "the ever-shifting * of"
- "in the ever-evolving"
- "stands out as a * marvel"
- "within this article"
- "within this * article"
- "within this guide"
- "within this * guide"
- "embark on a * adventure"
- "embark on an * adventure"
- "in today's * age"
- "in the digital"
- "this article is your"
- "this * travel guide"
- "the * tapestry of"
- "we invite you to * yourself"
- "adventure waiting to be"
- "whether you're a first-time"
- "pack your curiosity"
- "your key to unlocking the"
- "journey with us as we * the"
- "in this exploration of"
- "indulge in a * adventure"
- "in the realm of"
- "in the world of"
- "waiting to be unlocked"
- "beyond its * appearance lies"
- "beyond their * appearance lies"
- "let's deep dive"
- "join us on a * journey"
- "uncover the secrets"
- "this * adventure promises to"
- "this journey promises to"
- "as you embark on your own * adventures"
- "our * into the realm of"
- "as we conclude our journey"
- "through the * world of"
- "embark on your journey"
- "embark on your * journey"
- "embarking on your * journey"
- "embarking on the journey to"
- "navigate the * landscape"
- "navigating uncharted territory"
- "into the * world of"
- "entering the realm of"
- "in this guide, we will"
- "in this guide, we'll"
- "in the pages ahead"
- "you'll be equipped with the"
- "let's dive in"
- "embark on this next"
- "embark on this new"
- "gain valuable insights"
- "gained valuable insights"
- "navigate this journey"
- "armed with this knowledge"
- "embark on this * endeavor"
- "as we conclude our * guide"
- "the intricacies of"
- "with this newfound"
- "to wrap up"
- "stands at the forefront of"
- "* as a beacon of"
- "this * sets out to"
- "this * embarks on"
- "we stand on the * of an"
- "whether you're a veteran"
- "embark on this * journey"
- "whether you're a long-time"
- "in an * marked by"
- "throughout this article"
- "throughout this guide"
- "in closing"
- "throughout this exploration"
- "the * journey of"
- "the keys to unlocking"
- "in today's fast-paced"
- "the * journey to"
- "this article offers"
- "embark on your own * journey"
- "in today's * world"
- "the * journey towards"
- "as we conclude our"
- "embarked on a journey"
- "in wrapping up our"
- "journeyed through the"
- "continue on your * towards"
- "in concluding our * of"
- "by embracing this * approach"
- "embrace the journey"
- "to conclude our * of"
- "as you continue your journey towards"
- "we explore the * of"
- "we delve into the * of"
- "in this beginner's guide"
- "whether you're a novice"
- "this article aims to"
- "this guide aims to"
- "or a seasoned"
- "the evolving landscape of"
- "delving into the world of"
- "this beginner's guide aims to"
- "whether you're new to"
- "this guide offers"
- "the fast-paced * of"
- "* key to unlocking"
- "in this article, we explore the"
- "the hustle and bustle of"
- "embrace the * power of"
- "the tapestry of"
- "stands as a"
- "a delicate dance between"
- "transcends mere"
- "let's embrace the"
- "let us embrace the"
- "whether you're a beginner"
- "let's dive into the"
- "this guide is your passport"
- "navigate the * of"
- "navigating the * of"
- "this article delves into the"
- "reshaping the landscape of"
- "whether you're an experienced"
- "embarking on the * of"
- "in an era where"
- "this guide will"
- "this article will"
- "this guide is * to"
- "serve as * milestones"
- "outlined in this guide"

Avoid using antithesis constructions (e.g., “not X, but Y” or “not magic, just engineering”) — write statements directly instead of contrasting ideas for effect.

### Punctuation and Formatting
- Avoid using em dashes (--) in blog posts. Use regular dashes (-) or commas instead.
- Do not use "Conclusion" as a header. Instead, integrate the final thoughts naturally into the last section.

### General Writing Tips
- Be specific and concrete
- Use active voice when possible
- Choose strong verbs over weak verbs with adverbs
- Keep paragraphs short and focused
- Use bullet points for lists
- Include relevant examples and use cases
- Write for a technical audience while maintaining clarity

## How to Use This Guide

1. Before starting a new post, review this list
2. During editing, search your draft for these words/phrases
3. Replace them with more precise, effective alternatives
4. Run the validation script to check for AI-like phrases
5. Review the post for em dashes and conclusion headers

## Tips for Better Writing

- Be specific and concrete
- Use active voice when possible
- Choose strong verbs over weak verbs with adverbs

## Publishing Procedures

### How to Publish a Post

1. Ensure the post follows all writing guidelines
2. Run the validation script to check for AI-like phrases:
   ```bash
   ./scripts/validate-post.sh <post-path>
   ```
3. Publish the post using the publish script:
   ```bash
   ./scripts/publish_post.sh <post-path>
   ```

### Common Publishing Issues

- Do not use `npm run publish` as this project uses shell scripts for publishing
- Always validate posts before publishing
- Ensure all required images are in the correct assets directory
- Check that the post date is correct in the frontmatter

### Post Structure Requirements

- Frontmatter must include: title, date, description, tags, and cover_image
- Cover images should be placed in the corresponding year/month directory under assets
- All links should be relative to the content directory
- Code blocks should specify the language for syntax highlighting

## Documentation Maintenance

### When to Update Documentation

1. When encountering issues not covered in existing documentation
2. When discovering better ways to perform tasks
3. When finding common mistakes that others might make
4. When scripts or processes change
5. When adding new features or capabilities

### How to Update Documentation

1. Document the issue or discovery immediately
2. Include:
   - What was the problem
   - How it was discovered
   - The solution
   - Examples if applicable
3. Update relevant sections of existing documentation
4. Add new sections if the topic isn't covered

### Documentation Update Examples

- Path format clarification: Use relative paths from content/posts (e.g., `2025/aws-weekly-updates-april-2025.md`) instead of full paths
- Common errors and their solutions
- Script usage patterns and best practices
- New validation rules or requirements

### Documentation Review

- Review documentation updates for clarity and accuracy
- Ensure examples are current and working
- Remove outdated information
- Cross-reference related documentation sections