FROM node:18-alpine

WORKDIR /app

# Install dependencies early
COPY package*.json ./
RUN npm install

# Copy everything
COPY . .

# Expose Vite dev server port
EXPOSE 5173

# Start dev server
CMD ["npm", "run", "dev", "--", "--host"]
