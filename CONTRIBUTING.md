# Contributing to Rihla

Thank you for your interest in contributing to Rihla! This document provides guidelines and instructions for contributing.

## Code of Conduct

- Be respectful and inclusive
- No harassment or discrimination
- Constructive feedback only
- Report violations to maintainers

## Getting Started

1. **Fork the repository**
   ```bash
   git clone https://github.com/yourusername/Rihla.git
   cd Rihla
   ```

2. **Create a branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Install dependencies**
   ```bash
   cd backend && npm install
   cd ../admin && npm install
   cd ../company && npm install
   cd ../customer && npm install
   ```

4. **Set up environment**
   ```bash
   cp backend/.env.example backend/.env
   # Edit .env with your local values
   ```

## Development Workflow

### Before Coding

1. Check existing issues/PRs to avoid duplicates
2. Create an issue first for large features
3. Discuss approach in the issue
4. Get approval before starting work

### While Coding

1. **Follow the code style**
   - Run `npm run format` to auto-format
   - Run `npm run lint --fix` to fix lint issues
   - Use meaningful variable names
   - Add comments for complex logic

2. **Write tests**
   - Write unit tests for new functions
   - Update integration tests if needed
   - Ensure `npm run test` passes
   - Aim for 80%+ code coverage

3. **Keep commits clean**
   ```bash
   git add .
   git commit -m "feat(module): clear description of change"
   ```

4. **Commit message format**
   ```
   <type>(<scope>): <subject>

   <body>

   <footer>
   ```

   Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`
   
   Examples:
   - `feat(auth): add JWT token refresh endpoint`
   - `fix(booking): resolve race condition in ticket allocation`
   - `docs(setup): update installation instructions`

### Before Submitting PR

1. **Ensure code quality**
   ```bash
   npm run lint
   npm run format
   npm run test
   ```

2. **Update documentation**
   - Add/update README if needed
   - Document new environment variables
   - Update API docs in code comments

3. **Verify no conflicts**
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

## Pull Request Process

1. **Create PR with clear description**
   - Reference related issues: `Fixes #123`
   - Describe what changed and why
   - Include test coverage info

2. **PR Title Format**
   ```
   [type] Description of change
   ```
   Examples:
   - `[feature] Add payment gateway integration`
   - `[fix] Resolve booking confirmation email issue`
   - `[docs] Update deployment guide`

3. **PR Checklist**
   - [ ] Code follows style guide
   - [ ] All tests pass (`npm run test`)
   - [ ] New tests added for new features
   - [ ] Documentation updated
   - [ ] No breaking changes (or clearly documented)
   - [ ] Environment variables documented

4. **Review process**
   - Maintainers will review within 48 hours
   - Address review comments promptly
   - Push updates to same branch (keep PR open)
   - Rebase if needed to keep history clean

## Testing Requirements

### Unit Tests
```bash
cd backend
npm run test
```

### Coverage
```bash
npm run test:cov
```

### Running Specific Tests
```bash
npm run test -- --testNamePattern="description"
```

### Test Structure
```typescript
describe('FeatureName', () => {
  it('should do something', () => {
    // Arrange
    const input = { /* test data */ };
    
    // Act
    const result = functionUnderTest(input);
    
    // Assert
    expect(result).toBe(expected);
  });
});
```

## Code Style Guide

### TypeScript
- Use strict mode (enabled in `tsconfig.json`)
- Add explicit return types
- Avoid `any` type - use proper typing
- Use interfaces for object shapes

### Naming Conventions
- Classes: `PascalCase` (e.g., `UserService`)
- Functions/variables: `camelCase` (e.g., `getUserById`)
- Constants: `UPPER_SNAKE_CASE` (e.g., `MAX_RETRY_COUNT`)
- Files: `kebab-case.ts` (e.g., `user.service.ts`)

### Example
```typescript
interface User {
  id: string;
  email: string;
  isActive: boolean;
}

export class UserService {
  async getUserById(userId: string): Promise<User | null> {
    return this.userRepository.findOne(userId);
  }
}
```

## Documentation

### Code Comments
- Add JSDoc for public functions
- Explain 'why' not 'what'
- Keep comments up-to-date

```typescript
/**
 * Validates user email format
 * @param email - The email to validate
 * @returns true if email is valid, false otherwise
 * @throws InvalidEmailError if email format is incorrect
 */
export function validateEmail(email: string): boolean {
  // Implementation
}
```

### README Updates
- Add feature to features list
- Update setup instructions if needed
- Document new environment variables
- Add examples for new APIs

## Reporting Issues

When reporting bugs:

1. **Describe the issue clearly**
   - What were you trying to do?
   - What happened?
   - What did you expect to happen?

2. **Provide reproduction steps**
   ```
   1. Start the application
   2. Navigate to...
   3. Click...
   4. Observe the issue
   ```

3. **Include environment info**
   - OS and Node.js version
   - Relevant .env settings (without secrets)
   - Browser (if frontend issue)

4. **Attach logs or screenshots**
   - Terminal output
   - Browser console errors
   - Error stack traces

## Feature Requests

When suggesting features:

1. **Describe the use case**
   - Who needs this?
   - What problem does it solve?

2. **Suggest implementation**
   - How might it work?
   - What are alternatives?

3. **Consider impact**
   - Performance implications?
   - Breaking changes?
   - Dependencies needed?

## Coding Best Practices

### Architecture
- Keep services small and focused (Single Responsibility)
- Use dependency injection
- Avoid circular dependencies
- Keep layers separated (controllers → services → repositories)

### Error Handling
```typescript
try {
  const user = await this.userService.findById(id);
  if (!user) {
    throw new NotFoundException('User not found');
  }
} catch (error) {
  this.logger.error(`Error finding user: ${error.message}`);
  throw new InternalServerErrorException();
}
```

### Database Queries
- Use Prisma's select/include for efficiency
- Avoid N+1 queries
- Add proper indexing for frequently queried fields

## Security Considerations

When contributing, keep security in mind:

- Never log sensitive data (passwords, tokens)
- Validate all user inputs
- Use parameterized queries (Prisma does this)
- Keep dependencies updated
- Don't commit secrets (.env files)
- Use HTTPS in production
- Add CSRF protection where needed
- Sanitize HTML output

## Questions?

- Check existing documentation in `/docs`
- Look at similar code for patterns
- Ask in GitHub Discussions
- Email maintainers for sensitive issues

## Recognition

Contributors will be recognized in:
- README contributors list
- Release notes
- GitHub graphs
- Community celebrations

Thank you for contributing to Rihla! 🎉
