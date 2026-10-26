FROM public.ecr.aws/lambda/nodejs:20

COPY app.js package.json package-lock.json ./

# Reproducible, production-only install (excludes devDependencies like mocha/chai)
RUN npm ci --omit=dev

# Command can be overwritten by providing a different command in the template directly.
CMD ["app.lambdaHandler"]
