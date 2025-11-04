# 🤖 GitHub Automation Template

Template completo com automação GitHub Actions.

## ✅ Features

- **Auto-Merge**: PRs com label `auto-merge` + 1 approval
- **Auto-Fix**: ESLint & Prettier automático
- **Auto-Conflict-Resolve**: Resolve conflitos automaticamente
- **Auto-Issue**: Cria issues de TODOs no código
- **Auto-Copilot-Fix**: Label `copilot-fix` para fixes automáticos
- **Dependabot**: Updates semanais de dependências

## 🛠️ Setup

1. **Use este template** no GitHub
2. **Configure Branch Protection**: Settings > Branches > Add rule para `main`
   - Require 1 approval
   - Allow auto-merge
3. **Enable Actions**: Settings > Actions > Read and write permissions

## 📖 Uso

### Auto-Merge
```bash
gh pr edit <NUM> --add-label "auto-merge"
```

### Auto-Issue
Adicione no código:
```javascript
// TODO: Implementar feature X
// FIXME: Bug no módulo Y
```

### Auto-Copilot-Fix
```bash
gh issue edit <NUM> --add-label "copilot-fix"
```

## 📄 Licença

MIT License

---

**Automação completa com GitHub Actions** 🚀
