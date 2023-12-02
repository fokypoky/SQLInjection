<h1>Практическая работа №4</h1>
<h3>Задание №1. 1.Необходимо найти участок кода, содержащий инъекцию SQL кода в задании Blind Sql Injection на сайте dvwa.local с использованием статического анализатора кода (Можно использовать официальный ресурс или виртуальную машину Web Security Dojo)</h3>
Выполнено в WEB-версии sonarcloud.io
<pre>
if( isset( $_GET[ 'Submit' ] ) ) {
        // Get input
        $id = $_GET[ 'id' ];

        // Check database
        $getid  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
        $result = mysqli_query($GLOBALS["___mysqli_ston"],  $getid ); // Removed 'or die' to suppress mysql errors

        // Get results
        $num = @mysqli_num_rows( $result ); // The '@' character suppresses errors
        if( $num > 0 ) {
                // Feedback for end user
                $html .= '<pre>User ID exists in the database.</pre>';
        }
        else {
                // User wasn't found, so the page wasn't!
                header( $_SERVER[ 'SERVER_PROTOCOL' ] . ' 404 Not Found' );

                // Feedback for end user
                $html .= '<pre>User ID is MISSING from the database.</pre>';
        }

        ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}
</pre>
<p>Результат статического анализа доступен по ссылке</p>
<pre>
https://github.com/fokypoky/BruteforceDVWA/blob/main/analyze-result.png
</pre>

<h3>Задание №2. Проанализировать код и сделать кодревью, указав слабые места</h3>
<ol>
<li>CWE-89. Код подвержен SQL инъекциям из-за того, что значение переменной id не было защищено</li>
<li>CWE-20. Отсутствует проверка на корректность ввода данных, что может привести к уязвимостям</li>
<li>CWE-311. Данные, передаваемые через URL, могут быть перехвачены, так как они не зашифрованы</li>
<li>CWE-862. Отсутствует проверка прав доступа к базе данных</li>
<li>CWE-807. Программа полагается на ненадежные входные данные (GET параметры)</li>
<li>CWE-307. Отсутствие защиты от неудачных попыток входа. Предоставляет возможность, например, перебора паролей</li>
<li>CWE-79. Не защищенный вывод. В выражениях вида $html .= "какой-то текст и разметка..." возникает уязвимость к XSS атакам - злоумыщленник может внедрить свой JavaScript код</li>
</ol>