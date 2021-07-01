/// <reference types="cypress" />

 describe('Sprint 3 Test', () => {

    Cypress.on('uncaught:exception', (err, runnable) => {
        console.log(err);
        return false;
    })

    // Verify that input tags are required
    it('sign_up_01 - Verify that input tags are required', () => {
        // STEPS TO DO
        // Visit the imgexp site
        cy.visit('http://imgexp.herokuapp.com', {setTimeout: 600000})
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        // Click SIGN UP button without input
        cy.get('#signUpSubmit').click()
        
        // CHECK RESULTS
        // Check that the current page is still the userLogin page
        cy.url().should('include', '/userLogin')
        cy.url().should('eq', 'http://imgexp.herokuapp.com/userLogin')
        // Check that error message is being output
        cy.contains('Email is required').should('exist')
        cy.contains('Password is required').should('exist')
        
    })

    // Verify that your email must have domain name after "@"
    it('sign_up_02 - Verify that your email must have domain name after "@"', () => {
        // STEPS TO DO
        // Visit the imgexp site
        cy.visit('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        // Enter the fields
        // Email must not have domain name after "@"
        cy.get('#signUpEmail').type('thinh@')
        cy.get('#signUpPassword').type('1234567')
        cy.get('#signUpRepassword').type('1234567', {force:true})
        // Click SIGN UP button to submit
        cy.get('#signUpSubmit').click({force:true})

        // CHECK RESULTS
        // Check that the current page is still the userLogin page
        cy.url().should('include', '/userLogin')
        cy.url().should('eq', 'http://imgexp.herokuapp.com/userLogin')
        // Check that error message is being output
        cy.contains('Email is invalid').should('exist')
    })

    // Verify that your email does not contain 2 dot(.) or 2 "@"
    it('sign_up_03 - Verify that your email does not contain 2 dot(.) or 2 "@"', () => {
        // STEPS TO DO
        // Visit the imgexp site
        cy.visit('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        // Enter the fields
        // Email must have two "@"
        cy.get('#signUpEmail').type('thinh@@gmail.com')
        cy.get('#signUpPassword').type('1234567')
        cy.get('#signUpRepassword').type('1234567', {force:true})
        // Click SIGN UP button to submit
        cy.get('#signUpSubmit').click({force:true})

        // CHECK RESULTS
        // Check that the current page is still the userLogin page
        cy.url().should('include', '/userLogin')
        cy.url().should('eq', 'http://imgexp.herokuapp.com/userLogin')
        // Check that error message is being output
        cy.contains('Email is invalid').should('exist')
    })

    // Verify that your password can only contain 50 characters at most
    it('sign_up_04 - Verify that your password can only contain 50 characters at most', () => {
        // STEPS TO DO
        // Visit the imgexp site
        cy.visit('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        // Enter the fields
        // Password must have more than 50 characters
        cy.get('#signUpEmail').type('thinh@gmail.com')
        cy.get('#signUpPassword').type('thinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinh')
        cy.get('#signUpRepassword').type('thinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinhthinh', {force:true})
        // Click SIGN UP button to submit
        cy.get('#signUpSubmit').click({force:true})

        // CHECK RESULTS
        // Check that the current page is still the userLogin page
        cy.url().should('include', '/userLogin')
        cy.url().should('eq', 'http://imgexp.herokuapp.com/userLogin')
        // Check that error message is being output
        cy.contains('Password is at most 50 characters').should('exist')
    })
    
    // Verify that your password can contain 6 characters at least
    it('sign_up_05 - Verify that your password can contain 6 characters at least', () => {
        // STEPS TO DO
        // Visit the imgexp site
        cy.visit('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        // Enter the fields
        // Password must have more than 50 characters
        cy.get('#signUpEmail').type('thinh@gmail.com')
        cy.get('#signUpPassword').type('thi')
        cy.get('#signUpRepassword').type('thi', {force:true})
        // Click SIGN UP button to submit
        cy.get('#signUpSubmit').click({force:true})

        // CHECK RESULTS
        // Check that the current page is still the userLogin page
        cy.url().should('include', '/userLogin')
        cy.url().should('eq', 'http://imgexp.herokuapp.com/userLogin')
        // Check that error message is being output
        cy.contains('Password is at least 6 characters').should('exist')
    })

    // Verify that your password can contain only alphanumeric, upper case, lowercase, undercore(_) and dash(-)
    it('sign_up_06 - Verify that your password can contain only alphanumeric, upper case, lowercase, undercore(_) and dash(-)', () => {
        // STEPS TO DO
        // Visit the imgexp site
        cy.visit('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        // Enter the fields
        // Password must ONLY alphanumeric, upper case, lowercase, undercore(_) and dash(-)
        cy.get('#signUpEmail').type('thinh@gmail.com')
        cy.get('#signUpPassword').type('thinh_Thinh-1234')
        cy.get('#signUpRepassword').type('thinh_Thinh-1234', {force:true})
        // Click SIGN UP button to submit
        cy.get('#signUpSubmit').click({force:true})

        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('Password is invalid').should('not.exist', {setTimeout: 600000})
        // Check that the current page is NOT the userLogin page
        cy.url().should('not.include', '/userLogin')
        cy.url().should('not.eq', 'http://imgexp.herokuapp.com/userLogin')        
    })

    it('sign_up_07 - Verify that your password can contain only alphanumeric, upper case, lowercase, undercore(_) and dash(-)', () => {
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
         cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        // Enter the fields
        // Password must ONLY alphanumeric, upper case, lowercase, undercore(_) and dash(-)
        cy.get('#signUpEmail').type('thinh@gmail.com')
        cy.get('#signUpPassword').type('buithinh@#$%')
        cy.get('#signUpRepassword').type('buithinh@#$%', {force:true})
        // Click SIGN UP button to submit
        cy.get('#signUpSubmit').click({force:true})
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('Password cannot contain special character(s)').should('not.exist', {setTimeout: 600000})
        // Check that the current page is NOT the userLogin page
        cy.url().should('not.include', '/userLogin')
        cy.url().should('not.eq', 'http://imgexp.herokuapp.com/userLogin') 

 
    })

    it('sign_up_08 - Verify that your email is already in use', () => {
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
         cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
         // Enter the fields
        // Verify that your email is already in use
        cy.get('#signUpEmail').type('thinh@gmail.com')
        cy.get('#signUpPassword').type('thinh123456789')
        cy.get('#signUpRepassword').type('thinh123456789', {force:true})
        // Click SIGN UP button to submit
        cy.get('#signUpSubmit').click({force:true})
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('Existed email').should('not.exist', {setTimeout: 600000})
        // Check that the current page is NOT the userLogin page
        cy.url().should('not.include', '/userLogin')
        cy.url().should('not.eq', 'http://imgexp.herokuapp.com/userLogin')  
    })

    it('sign_in_01 - Verify that input  tags are required', () => {
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        // Click SIGN IN of user sign in
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('')
        cy.get('#signInPassword').type('')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('is required').should('not.exist', {setTimeout: 600000})
        // Check that the current page is NOT the userSignIn page
        cy.url().should('not.include', '/userLogin')
        cy.url().should('not.eq', 'http://imgexp.herokuapp.com/userLogin')  

    })

    it('sign_in_02 - Verify that your email must have domain name after "@"', () => {
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('thinh@')
        cy.get('#signInPassword').type('thinh123456789')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('Email is Invalid').should('not.exist', {setTimeout: 600000})
        // Check that the current page is NOT the userSignIn page
        cy.url().should('not.include', '/userLogin')
        cy.url().should('not.eq', 'http://imgexp.herokuapp.com/userLogin')  

    })

    it('sign_in_03 - Verify that the site do not accept incorrect account input', () =>{
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('thinh123@gmail.com')
        cy.get('#signInPassword').type('12')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('Invalid email or password').should('not.exist', {setTimeout: 600000})
        // Check that the current page is NOT the userSignIn page
        cy.url().should('not.include', '/userLogin')
        cy.url().should('not.eq', 'http://imgexp.herokuapp.com/userLogin')  
    })

    it('sign_in_04 - Password must be distinguished from upper and lower case', () =>{
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('thinh123@gmail.com')
        cy.get('#signInPassword').type('Thinh123456789')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('Invalid email or password').should('not.exist', {setTimeout: 600000})
        // Check that the current page is NOT the userSignIn page
        cy.url().should('not.include', '/userLogin')
        cy.url().should('not.eq', 'http://imgexp.herokuapp.com/userLogin')  
    })

    it(' Change password_01 - Verify that your old password is correct to change the password', () =>{
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('thinh12345@gmail.com')
        cy.get('#signInPassword').type('123456789')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.dropdown > button > i').click()
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.show.dropdown > ul > li:nth-child(1) > a').click()
        cy.get('body > app-root > app-user-profile > div > app-settings > p-tabmenu > div > ul > li:nth-child(2) > a > span.p-menuitem-text.ng-star-inserted').click()
        cy.get('body > app-root > app-change-password > div > div > div > form > div:nth-child(3) > p-password > div > input').type('123456')
        cy.get('body > app-root > app-change-password > div > div > div > form > div.input-field.error-box > p-password > div > input').type('0898313744')
        cy.get('body > app-root > app-change-password > div > div > div > form > div:nth-child(5) > p-password > div > input').type('0898313744')
        cy.get('body > app-root > app-change-password > div > div > div > form > input').click({force:true})
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('Password is incorrect').should('not.exist', {setTimeout: 600000})

    })

    it(' Change password_02 - Verify that input tags are required', () => {
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('thinh12345@gmail.com')
        cy.get('#signInPassword').type('123456789')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.dropdown > button > i').click()
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.show.dropdown > ul > li:nth-child(1) > a').click()
        cy.get('body > app-root > app-user-profile > div > app-settings > p-tabmenu > div > ul > li:nth-child(2) > a > span.p-menuitem-text.ng-star-inserted').click()
        cy.get('body > app-root > app-change-password > div > div > div > form > div:nth-child(3) > p-password > div > input').type('')
        cy.get('body > app-root > app-change-password > div > div > div > form > div.input-field.error-box > p-password > div > input').type('')
        cy.get('body > app-root > app-change-password > div > div > div > form > div:nth-child(5) > p-password > div > input').type('')
        cy.get('body > app-root > app-change-password > div > div > div > form > input').click({force:true})
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('is required').should('not.exist', {setTimeout: 600000})
    })

    it(' Change password_03- Verify that "Confirm Password" needs to be the same as "New Password"', () => {
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('thinh12345@gmail.com')
        cy.get('#signInPassword').type('123456789')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.dropdown > button > i').click()
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.show.dropdown > ul > li:nth-child(1) > a').click()
        cy.get('body > app-root > app-user-profile > div > app-settings > p-tabmenu > div > ul > li:nth-child(2) > a > span.p-menuitem-text.ng-star-inserted').click()
        cy.get('body > app-root > app-change-password > div > div > div > form > div:nth-child(3) > p-password > div > input').type('123456789')
        cy.get('body > app-root > app-change-password > div > div > div > form > div.input-field.error-box > p-password > div > input').type('tym882018')
        cy.get('body > app-root > app-change-password > div > div > div > form > div:nth-child(5) > p-password > div > input').type('tym8820')
        cy.get('body > app-root > app-change-password > div > div > div > form > input').click({force:true})
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('New repassword is not match with new password').should('not.exist', {setTimeout: 600000})

    })

    it(' Change password_04 - Verify that "Confirm Password" and "New Password" contain 50 characters at most', () => {
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('thinh12345@gmail.com')
        cy.get('#signInPassword').type('123456789')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
        // click nut 
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.dropdown > button > i').click()
        // Click settings
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.show.dropdown > ul > li:nth-child(1) > a').click()
        //click change-password
        cy.get('body > app-root > app-user-profile > div > app-settings > p-tabmenu > div > ul > li:nth-child(2) > a > span.p-menuitem-text.ng-star-inserted').click()
        // change-password
        cy.get('body > app-root > app-change-password > div > div > div > form > div:nth-child(3) > p-password > div > input').type('123456789')
        cy.get('body > app-root > app-change-password > div > div > div > form > div.input-field.error-box > p-password > div > input').type('admin1111111111111111111111111111111111111111111111111111111')
        cy.get('body > app-root > app-change-password > div > div > div > form > div:nth-child(5) > p-password > div > input').type('admin1111111111111111111111111111111111111111111111111111111')
        cy.get('body > app-root > app-change-password > div > div > div > form > input').click({force:true})
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('Your Password is at most 50 characters').should('not.exist', {setTimeout: 600000})
    })

    it(' Change name - Verify that your user name can contain 50 characters at most', () => {
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('thinh12345@gmail.com')
        cy.get('#signInPassword').type('123456789')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
         // click nut 
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.dropdown > button > i').click()
        // Click settings
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.show.dropdown > ul > li:nth-child(1) > a').click()
        // Click edit profile
        cy.get('body > app-root > app-user-profile > div > app-settings > p-tabmenu > div > ul > li:nth-child(1) > a > span.p-menuitem-text.ng-star-inserted').click()
        //change name
        cy.get('body > app-root > app-user-profile > div > div > div > form > div.input-field.error-box > input').type('thinhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh')
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('Your name is at most 50 characters').should('not.exist', {setTimeout: 600000})
        // click save
        cy.get('body > app-root > app-user-profile > div > div > div > form > input').click({force:true})
    })

    it(' Change name_ 02 - Verify that input  tags are required', () => {
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('thinh12345@gmail.com')
        cy.get('#signInPassword').type('123456789')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
         // click nut 
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.dropdown > button > i').click()
        // Click settings
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.show.dropdown > ul > li:nth-child(1) > a').click()
        // Click edit profile
        cy.get('body > app-root > app-user-profile > div > app-settings > p-tabmenu > div > ul > li:nth-child(1) > a > span.p-menuitem-text.ng-star-inserted').click()
        //change name
        cy.get('body > app-root > app-user-profile > div > div > div > form > div.input-field.error-box > input').type('')
        // CHECK RESULTS
        // Check that error message is NOT being output
        cy.contains('Username is required').should('not.exist', {setTimeout: 600000})
        // click save
        cy.get('body > app-root > app-user-profile > div > div > div > form > input').click({force:true})
    })

    it(' Change name_ 03 -  Verify that name input support all language', () => {
        //STEPS TO DO 
        //Visit the imgexp site
        cy.viewport('http://imgexp.herokuapp.com')
        // Click Sign-in button
        cy.get('body > app-root > app-home > app-header > div > header > a.btn.btn-danger.rounded-pill.sign-in').click()
        cy.get('#sign-in-btn').click();
        cy.get('#signInEmail').type('thinh12345@gmail.com')
        cy.get('#signInPassword').type('123456789')
        // Click SIGN UP button to submit
        cy.get('#signInSubmit').click({force:true})
         // click nut 
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.dropdown > button > i').click()
        // Click settings
        cy.get('body > app-root > app-home > app-header > div > header > div.btn-group.drop-menu.btn-sm.ng-star-inserted.show.dropdown > ul > li:nth-child(1) > a').click()
        // Click edit profile
        cy.get('body > app-root > app-user-profile > div > app-settings > p-tabmenu > div > ul > li:nth-child(1) > a > span.p-menuitem-text.ng-star-inserted').click()
        //change name
        cy.get('body > app-root > app-user-profile > div > div > div > form > div.input-field.error-box > input').type('thịnh')
        // click save
        cy.get('body > app-root > app-user-profile > div > div > div > form > input').click({force:true})   
    })

  
})
