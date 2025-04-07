# Create project folders
```
mkdir my-app && cd my-app
mkdir backend frontend
```

# Initialize backend
```
cd backend
echo "Initializing backend..."
npm init -y
npm install express cors dotenv @supabase/supabase-js
npm install -D typescript ts-node nodemon @types/node @types/express @types/cors
```

```
echo "Setting up TypeScript..."
cat <<EOT > tsconfig.json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "CommonJS",
    "outDir": "dist",
    "rootDir": "src",
    "strict": true
  }
}
EOT
```

```
mkdir src
touch src/index.ts
cat <<EOT > src/index.ts
import express from 'express';
import cors from 'cors';
import { createClient } from '@supabase/supabase-js';
import dotenv from 'dotenv';

dotenv.config();
const app = express();
const port = process.env.PORT || 5000;

const supabase = createClient(process.env.SUPABASE_URL!, process.env.SUPABASE_KEY!);

app.use(cors());
app.use(express.json());

app.get('/', (req, res) => {
  res.send('Express + TypeScript Server');
});

app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
EOT
```

```
touch .env
cat <<EOT > .env
SUPABASE_URL=
SUPABASE_KEY=
EOT
```

```
echo "Setting up Nodemon..."
cat <<EOT > package.json
{
  "scripts": {
    "dev": "nodemon src/index.ts",
    "start": "node dist/index.js"
  }
}
EOT
```

# Initialize frontend
```
cd ../frontend
echo "Initializing frontend..."
npm create vite@latest . -- --template react-ts
npm install
npm install @supabase/supabase-js
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```
cat <<EOT > tailwind.config.ts
import type { Config } from 'tailwindcss';

const config: Config = {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}"
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};

export default config;
EOT
```

```
cat <<EOT > src/main.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App.tsx';
import './index.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
EOT
```

```
cat <<EOT > src/App.tsx
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(import.meta.env.VITE_SUPABASE_URL, import.meta.env.VITE_SUPABASE_KEY);

function App() {
  return (
    <div className="h-screen flex items-center justify-center bg-gray-100">
      <h1 className="text-3xl font-bold">React + Vite + Supabase</h1>
    </div>
  );
}

export default App;
EOT
```

```
touch .env
cat <<EOT > .env
VITE_SUPABASE_URL=
VITE_SUPABASE_KEY=
EOT
```

```
# Final instructions
echo "Setup complete! To start the project, run:"
echo "cd backend && npm run dev"
```
