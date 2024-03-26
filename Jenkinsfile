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
            if (!fileExists('gh-pages-temp')) {
                bat 'mkdir gh-pages-temp'
            }
        }

        // Copy static files to a temporary directory
        bat 'xcopy /s /Y out gh-pages-temp' // Add the /Y option here

        // Exclude node_modules from being added to the repository
        bat 'echo node_modules/ >> .gitignore'

        // Create or switch to gh-pages branch
        bat 'git checkout -b gh-pages || git checkout gh-pages'

        // Clear contents of gh-pages branch
        bat 'git rm -rf .'

        // Commit empty state
        bat 'git commit -m "Initial empty commit for GitHub Pages"'

        // Copy static files to the root directory
        bat 'xcopy /s /Y gh-pages-temp .' // Add the /Y option here

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
