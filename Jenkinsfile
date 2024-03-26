pipeline {
    environment {
        imagename = "raj0215/blogpost"
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git(url: 'https://github.com/worldOfRavi/blogpost.git', branch: 'main')
            }
        }
        stage('Building Next.js project') {
            steps {
                bat 'npm install' // Install dependencies using bat (Windows shell)
                bat 'npm run build' // Build Next.js project using bat
            }
        }
        stage('Deploy to GitHub Pages') {
            steps {
                script {
                    // Ensure temporary directory exists
                    bat 'mkdir gh-pages-temp'
                }

                // Copy static files to a temporary directory
                bat 'xcopy /s out gh-pages-temp'

                // Exclude node_modules from being added to the repository
                bat 'echo node_modules/ >> .gitignore'

                // Delete gh-pages branch if it exists
                bat 'git branch -D gh-pages || true'

                // Create gh-pages branch
                bat 'git checkout -b gh-pages'

                // Clear contents of gh-pages branch
                bat 'git rm -rf .'

                // Commit empty state
                bat 'git commit -m "Initial empty commit for GitHub Pages"'

                // Copy static files to the root directory
                bat 'xcopy /s gh-pages-temp .'

                // Exclude node_modules from being added to the repository
                bat 'echo node_modules/ >> .gitignore'

                // Add, commit, and push to GitHub Pages branch
                bat 'git add .'
                bat 'git commit -m "Deploy to GitHub Pages"'
                bat 'git push origin gh-pages --force'

                // Clean up temporary files and switch back to main branch
                bat 'rmdir /s /q gh-pages-temp'
                bat 'git checkout main'
            }
        }
    }
}