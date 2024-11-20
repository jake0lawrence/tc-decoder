# T&C Decoder Tool

**Simplify and understand complex terms and conditions with ease.**

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Demo](#demo)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Scripts](#scripts)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

---

## Introduction

The **T&C Decoder Tool** is a web application designed to help users easily understand the complex language often found in terms and conditions documents. By leveraging the power of AI through OpenAI's GPT models, the tool analyzes and summarizes lengthy legal texts into plain, understandable language.

---

## Features

- **Upload Documents**: Users can upload terms and conditions documents in various formats (PDF, DOCX, TXT).
- **AI-Powered Analysis**: Utilizes OpenAI's GPT models to generate simplified summaries.
- **Interactive Summaries**: Provides collapsible cards and visualizations for easy navigation of key points.
- **No Data Retention**: Ensures user privacy by not storing any uploaded documents or personal data.
- **User Authentication**: Secure registration and login system using JWT.
- **Responsive Design**: Accessible on desktops, tablets, and mobile devices.
- **Accessibility Compliance**: Adheres to WCAG 2.1 guidelines for an inclusive user experience.

---

## Demo

*Note: If a live demo is available, provide the link here. If not, consider adding screenshots of the application interface.*

---

## Technology Stack

### Frontend

- **React.js**: JavaScript library for building user interfaces.
- **Context API and Hooks**: State management within React.
- **Axios**: Promise-based HTTP client for API calls.
- **React Router**: Navigation within the application.
- **CSS Modules**: Styling components.

### Backend

- **Node.js**: JavaScript runtime environment.
- **Express.js**: Web application framework for Node.js.
- **Sequelize**: Promise-based ORM for PostgreSQL.
- **JWT (jsonwebtoken)**: Authentication via JSON Web Tokens.
- **OpenAI API**: Integration with GPT models for AI processing.

### Database

- **PostgreSQL**: Relational database management system.

### Others

- **Nginx**: Web server and reverse proxy.
- **PM2**: Process manager for Node.js applications.
- **GitHub Actions**: CI/CD pipeline for automated testing and deployment.
- **Let's Encrypt**: SSL/TLS certificates for secure connections.

---

## Prerequisites

- **Node.js**: v14.x or higher
- **npm**: v6.x or higher
- **PostgreSQL**: v12.x or higher
- **Git**: Version control system
- **OpenAI API Key**: Obtain from [OpenAI](https://openai.com/)

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/tc-decoder-tool.git
cd tc-decoder-tool
```

### 2. Install Dependencies

#### Frontend

```bash
cd frontend
npm install
```

#### Backend

```bash
cd ../backend
npm install
```

### 3. Set Up the Database

- Ensure PostgreSQL is installed and running.
- Create a new database:

```bash
createdb tc_decoder_db
```

---

## Configuration

### Backend Environment Variables

Create a `.env` file in the `backend` directory with the following content:

```env
NODE_ENV=development
PORT=3001
DATABASE_URL=postgres://username:password@localhost:5432/tc_decoder_db
JWT_SECRET=your_jwt_secret
OPENAI_API_KEY=your_openai_api_key
```

- Replace `username` and `password` with your PostgreSQL credentials.
- Replace `your_jwt_secret` with a secure secret key.
- Replace `your_openai_api_key` with your OpenAI API key.

### Frontend Environment Variables

Create a `.env` file in the `frontend` directory with the following content:

```env
REACT_APP_API_URL=http://localhost:3001/api
```

---

## Usage

### 1. Run the Backend Server

Navigate to the `backend` directory and run:

```bash
npm run dev
```

- The backend server will start on `http://localhost:3001`.

### 2. Run the Frontend Application

Navigate to the `frontend` directory and run:

```bash
npm start
```

- The frontend application will open in your default browser at `http://localhost:3000`.

### 3. Access the Application

- Register a new user or log in with your credentials.
- Upload a terms and conditions document for analysis.
- View the simplified summary and interact with the results.

---

## Project Structure

```
tc-decoder-tool/
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── contexts/
│   │   ├── hooks/
│   │   ├── pages/
│   │   ├── App.js
│   │   └── index.js
│   ├── .env
│   ├── package.json
│   └── ...
├── backend/
│   ├── controllers/
│   ├── middlewares/
│   ├── models/
│   ├── routes/
│   ├── services/
│   ├── utils/
│   ├── app.js
│   ├── server.js
│   ├── .env
│   ├── package.json
│   └── ...
├── README.md
└── LICENSE
```

---

## Scripts

### Frontend

- **Start Development Server**: `npm start`
- **Build for Production**: `npm run build`
- **Run Tests**: `npm test`
- **Lint Code**: `npm run lint`

### Backend

- **Start Development Server**: `npm run dev`
- **Run in Production**: `npm start`
- **Run Tests**: `npm test`
- **Run Migrations**: `npx sequelize-cli db:migrate`
- **Seed Database**: `npx sequelize-cli db:seed:all`
- **Lint Code**: `npm run lint`

---

## Contributing

We welcome contributions to the T&C Decoder Tool! Here's how you can help:

1. **Fork the Repository**: Click the "Fork" button at the top right of this page.
2. **Clone Your Fork**: Clone your forked repository to your local machine.

   ```bash
   git clone https://github.com/yourusername/tc-decoder-tool.git
   ```

3. **Create a Branch**: Create a new branch for your feature or bug fix.

   ```bash
   git checkout -b feature/your-feature-name
   ```

4. **Make Your Changes**: Implement your feature or fix.
5. **Commit Your Changes**: Commit your changes with a clear message.

   ```bash
   git commit -m "Add feature X"
   ```

6. **Push to Your Fork**:

   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create a Pull Request**: Go to the original repository and create a pull request.

### Guidelines

- Follow the existing code style and conventions.
- Write clear commit messages.
- Update documentation and comments where necessary.
- Ensure your code passes all tests.

---

## License

This project is licensed under the [MIT License](LICENSE).

---

## Contact

If you have any questions, suggestions, or feedback, feel free to reach out.

- **Email**: [contact@tcdecodertool.com](mailto:contact@tcdecodertool.com)
- **Issue Tracker**: [GitHub Issues](https://github.com/yourusername/tc-decoder-tool/issues)

---

*Thank you for using the T&C Decoder Tool! Your support helps us make legal documents more accessible to everyone.*
