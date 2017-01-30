# Website-Login
C# login code for a website.


   
protected void Login1_Authenticate(object sender, AuthenticateEventArgs e)

   
{

       
bool authenticated = this.ValidateCredentials(Login1.UserName,
Login1.Password);

 

       
if (authenticated)

       
{

            FormsAuthentication.RedirectFromLoginPage(Login1.UserName,
Login1.RememberMeSet);

       
}

   
}

   
public bool IsAlphaNumeric(string text)

   
{

       
return Regex.IsMatch(text, "^[a-zA-Z0-9]+$");

   
}

 

   
private bool ValidateCredentials(string userName, string password)

   
{

       
bool returnValue = false;

 

       
if (this.IsAlphaNumeric(userName)
&& userName.Length <= 50 && password.Length <= 50)

       
{

            SqlConnection conn = null;

 

            try

            {

     
          string sql = "select count(*) from
users where username = @username and password = @password";

 

                conn = new SqlConnection(ConfigurationManager.ConnectionStrings["MusicHitsConnectionString"].ConnectionString);

                SqlCommand cmd = new SqlCommand(sql, conn);

 

                SqlParameter user = new SqlParameter();

                user.ParameterName = "@username";

                user.Value = userName.Trim();

                cmd.Parameters.Add(user);

 

                SqlParameter pass = new SqlParameter();

                pass.ParameterName = "@password";

                //pass.Value =
Hasher.HashString(password.Trim());

                pass.Value = password.Trim();

                cmd.Parameters.Add(pass);

 

         
      conn.Open();

 

                int count = (int)cmd.ExecuteScalar();

 

                if (count > 0) returnValue = true;

            }

            catch
(Exception)

            {

                // Log your error

            }

            finally

   
        {

                if (conn != null) conn.Close();

            }

       
}

       
else

       
{

            // Log error - user name not alpha-numeric or 

            // username or password exceed the length limit!

       
}

 

       
return returnValue;

   
}

}

