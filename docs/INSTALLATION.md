# Installation Guide

## Quick Start

The GitHub Skill Finder is ready to use immediately upon installation.

## Prerequisites

- Claude with skill support enabled
- Internet access to search GitHub
- (Optional) A GitHub account for private repository access

## Installation Steps

### Method 1: Automatic Installation

1. Ask Claude: "Find me a skill for [your task]"
2. Claude will automatically use the GitHub Skill Finder
3. Follow the prompts to install any recommended skills

### Method 2: Manual Installation

1. Clone or download this repository
2. Copy the `SKILL.md` file to your Claude skills directory
3. Restart Claude
4. The skill will be available immediately

## Configuration

No configuration required. The skill works out of the box.

## Verification

To verify the skill is installed:

1. Ask Claude: "What skills do you have?"
2. Look for "github-skill-finder" in the response
3. Test with: "Find me a skill for data visualization"

## Troubleshooting

### Skill Not Found

- Ensure `SKILL.md` is in the correct directory
- Restart Claude after installation
- Check that the skill name matches exactly

### Search Returns No Results

- Try different search terms
- Use more specific keywords
- Check that you have internet access

### Cannot Access Private Repositories

- Provide a GitHub personal access token if needed
- Note that the skill will inform you if a repo is private

## Uninstallation

To remove the skill:

1. Delete the `SKILL.md` file from your skills directory
2. Restart Claude
3. The skill will no longer be available

## Support

For issues or questions:

1. Check the main README.md
2. Review the SKILL.md workflow documentation
3. Visit the GitHub repository: https://github.com/MalikBasharat/claude-skill-github-finder