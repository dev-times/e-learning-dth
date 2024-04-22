# React + TypeScript + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

## Expanding the ESLint configuration

If you are developing a production application, we recommend updating the configuration to enable type aware lint rules:

- Configure the top-level `parserOptions` property like this:

```js
export default {
  // other rules...
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: ['./tsconfig.json', './tsconfig.node.json'],
    tsconfigRootDir: __dirname,
  },
}
```

- Replace `plugin:@typescript-eslint/recommended` to `plugin:@typescript-eslint/recommended-type-checked` or `plugin:@typescript-eslint/strict-type-checked`
- Optionally add `plugin:@typescript-eslint/stylistic-type-checked`
- Install [eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react) and add `plugin:react/recommended` & `plugin:react/jsx-runtime` to the `extends` list

## Bước 1 - Khởi tạo dự án Vite
npm create vite@latest
Need to install the following packages:
  create-vite@4.1.0
Ok to proceed? (y) y
✔ Project name: … react-app
✔ Select a framework: › React
✔ Select a variant: › TypeScript + SWC
cd react-app
npm i
## Bước 2 - Cài các package liên quan ESLint và Prettier
##### npm i prettier eslint-config-prettier eslint-plugin-prettier -D
- prettier: code formatter chính
- eslint-config-prettier: Bộ config ESLint để vô hiệu hóa các rule của ESLint mà xung đột với Prettier.
- eslint-plugin-prettier: Dùng thêm 1 số rule Prettier cho ESLint
## Bước 3 - Config ESlint để chuẩn hóa code
Mở file .eslintrc.cjs lên
Thêm đoạn giá trị này vào mảng ignorePatterns, mục đích là mình không muốn ESLint check file vite.config.ts
'vite.config.ts'

Thêm đoạn code sau vào mảng extends
'eslint-config-prettier', 'prettier'

Thêm đoạn code sau vào mảng plugins
'prettier'

Thêm đoạn code sau vào object rules để thêm các rule của Prettier
'prettier/prettier': [
      'warn',
      {
        arrowParens: 'always',
        semi: false,
        trailingComma: 'none',
        tabWidth: 2,
        endOfLine: 'auto',
        useTabs: false,
        singleQuote: true,
        printWidth: 120,
        jsxSingleQuote: true
      }
    ]

## Bước 4 - Config Prettier để format code
Tạo file .prettierrc trong thư trong thư mục root với nội dung dưới đây
{
  "arrowParens": "always",
  "semi": false,
  "trailingComma": "none",
  "tabWidth": 2,
  "endOfLine": "auto",
  "useTabs": false,
  "singleQuote": true,
  "printWidth": 120,
  "jsxSingleQuote": true
}
Mục đích là cấu hình prettier. Anh em nên cài Extension Prettier - Code formatter cho VS Code để nó hiểu nhé.

## Tiếp theo Tạo file .prettierignore ở thư mục root
Mục đích là Prettier bỏ qua các file không cần thiết

node_modules/
dist/

## Bước 5 - Config editor để chuẩn hóa cấu hình editor
Tạo file .editorconfig ở thư mục root
Mục đích là cấu hình các config đồng bộ các editor với nhau nếu dự án có nhiều người tham gia.
Để VS Code hiểu được file này thì anh em cài Extension là EditorConfig for VS Code nhé

[*]
indent_size = 2
indent_style = space

## Bước 6 - Cấu hính alias cho tsconfig.json
Việc thêm alias vào file tsconfig.json sẽ giúp VS Code hiểu mà tự động import giúp chúng ta. Lưu ý cái này chỉ giúp
Thêm đoạn này vào compilerOptions trong file tsconfig.json

"baseUrl": ".",
"paths": {
  "~/*": ["src/*"]
}

==> Ý nghĩa của đoạn này là ta có thể import Login from '~/pages/Login' thay vì import Login from '../../pages/Login'. Ngắn gọn và dễ nhìn hơn nhiều!

## Bước 7 - Cấu hình alias cho vite vite.config.ts
Cài package @types/node để sử dụng node js trong file ts không bị lỗi

npm i @types/node -D

### Cấu hình alias và enable source map ở file vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react-swc'
import path from 'path'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {
    port: 3000
  },
  css: {
    devSourcemap: true
  },
  resolve: {
    alias: {
      '~': path.resolve(__dirname, './src')
    }
  }
})

## Bước 8 - Cập nhật script cho package.json
Mở file package.json lên, thêm đoạn script dưới vào

"scripts": {
    //...
    "lint:fix": "eslint --fix src --ext ts,tsx",
    "prettier": "prettier --check \"src/**/(*.tsx|*.ts|*.css|*.scss)\"",
    "prettier:fix": "prettier --write \"src/**/(*.tsx|*.ts|*.css|*.scss)\""
}


## Câu lệnh để chạy dự án
Đến đây là xong rồi đó, để chạy trong môi trường dev thì chúng ta sẽ chạy bằng câu lệnh npm run dev.
- Nếu muốn build thì npm run build
Ngoài ra còn có một số câu lệnh như
- Preview kết quả build bằng câu lệnh npm run preview
- Kiểm tra dự án có bị lỗi gì liên quan ESLint không: npm run lint
- Tự động fix các lỗi liên quan ESLint (không phải cái gì cũng fix được, nhưng fix cũng nhiều): npm run lint:fix
- Kiểm tra dự án có bị lỗi gì liên quan Prettier không: npm run prettier
- Tự động fix các lỗi liên quan Prettier: npm run prettier:fix