# Contributing to ADS-B Local Flight Tracking

Thank you for considering contributing to this project! Contributions from the community help make this integration better for everyone.

## How to Contribute

### Reporting Bugs

If you find a bug, please create an issue with:

1. **Clear title** describing the problem
2. **Home Assistant version** you're using
3. **Steps to reproduce** the issue
4. **Expected behavior** vs actual behavior
5. **Relevant logs** (from Settings → System → Logs)
6. **Configuration snippets** (remove sensitive info like IPs)

### Suggesting Features

Feature requests are welcome! Please include:

1. **Clear description** of the feature
2. **Use case** - why is this useful?
3. **Proposed implementation** (if you have ideas)
4. **Alternatives considered**

### Pull Requests

#### Before Submitting

1. **Search existing issues/PRs** to avoid duplicates
2. **Test your changes** thoroughly
3. **Update documentation** if needed
4. **Follow code style** (match existing code)

#### Submitting a PR

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/amazing-feature`)
3. **Make your changes**
4. **Test thoroughly**
5. **Commit with clear messages**
6. **Push to your fork**
7. **Open a Pull Request**

#### PR Guidelines

- **One feature per PR** - keep changes focused
- **Clear description** - explain what and why
- **Link related issues** - use "Fixes #123" or "Relates to #456"
- **Update CHANGELOG.md** - add your changes under [Unreleased]
- **Update docs** - if you change functionality

### Development Setup

1. Clone the repository
2. Test in your Home Assistant instance
3. Make changes
4. Verify everything works
5. Submit PR

### Code Style

- **YAML**: Follow Home Assistant YAML conventions
  - 2-space indentation
  - Comments for complex sections
  - Clear entity naming
  
- **Jinja Templates**:
  - Readable formatting
  - Comments for complex logic
  - Efficient filters

- **JavaScript** (for cards):
  - Clear variable names
  - Comments for calculations
  - Maintainable structure

### Testing Checklist

Before submitting, verify:

- [ ] REST sensor pulls data correctly
- [ ] Template sensors process data properly
- [ ] Automation triggers as expected
- [ ] Dashboard cards display correctly
- [ ] No errors in logs
- [ ] Documentation is updated
- [ ] CHANGELOG is updated

### Documentation

When changing functionality:

1. Update relevant markdown files
2. Add/update code comments
3. Update CHANGELOG.md
4. Consider adding troubleshooting entries

### What We're Looking For

**Priority contributions:**
- Bug fixes
- Performance improvements
- Documentation improvements
- New features (see CHANGELOG.md "Planned Features")
- Additional TTS platform support
- Metric unit improvements
- Dashboard card enhancements

**Examples:**
- Per-aircraft cooldown system
- Configurable announcement messages
- Military aircraft filtering
- Historical tracking
- Additional template sensors
- Card themes/customization

## Code of Conduct

### Be Respectful

- Welcome newcomers
- Be patient with questions
- Provide constructive feedback
- Assume good intentions

### Be Collaborative

- Share knowledge
- Help others learn
- Credit contributors
- Celebrate successes

### Be Professional

- Keep discussions on-topic
- Avoid personal attacks
- Respect different viewpoints
- Follow Home Assistant community standards

## Questions?

- **Issues**: Use GitHub Issues for bugs/features
- **Discussions**: Use GitHub Discussions for questions
- **Home Assistant**: Post in HA Community forums

## Recognition

Contributors will be:
- Listed in pull request history
- Mentioned in release notes
- Part of improving the project for everyone!

## License

By contributing, you agree that your contributions will be licensed under the MIT License (same as the project).

---

Thank you for contributing! 🎉
