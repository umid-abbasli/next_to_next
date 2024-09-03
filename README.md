<!-- // <?php
// // Set the content type to UTF-8
// header('Content-Type: text/html; charset=utf-8');

// // Database connection credentials
// $servername = "localhost";
// $username = "asenacars";
// $password = "sdcL}1AME3Cx";
// $dbname = "asenacars";

// // Create a new MySQLi connection
// $conn = new mysqli($servername, $username, $password, $dbname);

// // Set the connection character set to UTF-8
// $conn->set_charset("utf8");

// // Check for any connection errors
// if ($conn->connect_error) {
//     die("Connection failed: " . $conn->connect_error);
// }

// // Retrieve the GET parameters and sanitize them
// $selectedBrand = isset($_GET['brand']) ? trim($_GET['brand']) : '';
// $selectedModel = isset($_GET['model']) ? trim($_GET['model']) : '';
// $selectedEngineCapacity = isset($_GET['engine-capacity']) ? trim($_GET['engine-capacity']) : '';
// $selectedYear = isset($_GET['year']) ? trim($_GET['year']) : '';
// $selectedColor = isset($_GET['color']) ? trim($_GET['color']) : '';
// $sortOrder = isset($_GET['sort']) ? trim($_GET['sort']) : '';

// // Debug: Output parameters to check if they are correctly retrieved
// echo "<pre>";
// echo "Brand: $selectedBrand\n";
// echo "Model: $selectedModel\n";
// echo "Engine Capacity: $selectedEngineCapacity\n";
// echo "Year: $selectedYear\n";
// echo "Color: $selectedColor\n";
// echo "Sort Order: $sortOrder\n";
// echo "</pre>";

// // Determine the sort order
// $sortColumn = "xfields"; // Assuming price is stored in xfields
// $sortDirection = "ASC";

// if ($sortOrder === 'high-to-low') {
//     $sortDirection = "DESC";
// } elseif ($sortOrder === 'low-to-high') {
//     $sortDirection = "ASC";
// }

// // Prepare the SQL statement with LIKE clauses to search within the xfields column
// $query = "
//     SELECT title, xfields, date, alt_name, id, short_story
//     FROM dle_post 
//     WHERE category = 3 
//     AND xfields LIKE ? 
//     AND xfields LIKE ? 
//     AND xfields LIKE ? 
//     AND xfields LIKE ? 
//     AND xfields LIKE ?
//     ORDER BY 
//     CASE 
//         WHEN xfields LIKE '%price|' THEN 
//             CAST(SUBSTRING_INDEX(SUBSTRING_INDEX(xfields, 'price|', -1), '||', 1) AS DECIMAL(10, 2))
//         ELSE
//             0
//     END $sortDirection
// ";

// $stmt = $conn->prepare($query);

// // Construct the search patterns
// $brandPattern = "%brand|{$selectedBrand}|%";
// $modelPattern = "%model|{$selectedModel}|%";
// $engineCapacityPattern = "%engine-capacity|{$selectedEngineCapacity}|%";
// $yearPattern = "%year-of-car|{$selectedYear}|%";
// $colorPattern = "%color|{$selectedColor}|%";

// // Bind the search patterns to the prepared statement
// $stmt->bind_param("sssss", $brandPattern, $modelPattern, $engineCapacityPattern, $yearPattern, $colorPattern);

// // Execute the prepared statement
// if (!$stmt->execute()) {
//     die("Query execution failed: " . $stmt->error);
// }

// // Get the result set from the executed statement
// $result = $stmt->get_result();

// echo '<div class="cars__inner">';

// // Check if any results were found
// if ($result->num_rows > 0) {
//     // Iterate through each result
//     while ($row = $result->fetch_assoc()) {
//         // Parse the xfields into an array
//         $xfieldsArray = [];
//         $fields = explode("||", $row["xfields"]);
//         foreach ($fields as $field) {
//             list($key, $value) = explode("|", $field);
//             $xfieldsArray[$key] = $value;
//         }

//         // Build the full link based on the alt_name (assuming it's used for the URL)
//         $fullLink = "/announcement/" . htmlspecialchars($row['id'], ENT_QUOTES, 'UTF-8') . "-" . htmlspecialchars($row['alt_name'], ENT_QUOTES, 'UTF-8') . ".html";

//         // Create a new DOMDocument object and load the short_story HTML
//         $dom = new DOMDocument();
//         @$dom->loadHTML($row['short_story']); // Use @ to suppress warnings caused by malformed HTML

//         // Get the first <img> tag
//         $imgTag = $dom->getElementsByTagName('img')->item(0);

//         // Get the image source if available
//         $imgSrc = $imgTag ? $imgTag->getAttribute('src') : 'https://turbo.azstatic.com/uploads/f460x343/2024%2F06%2F28%2F20%2F52%2F14%2Fc531fcef-b910-400a-88ad-70e5cecb8027%2F73879_Llsf7HE7T9ZVmMOurKtMGQ.jpg';

//         // Clean up the URL and remove any extra quotes
//         $imgSrc = stripslashes(htmlspecialchars_decode($imgSrc));
//         $imgSrc = trim($imgSrc, '"'); // Remove extra quotes around the URL

//         // Display the result with an image
//         echo '<a href="' . $fullLink . '" class="cars__item">
//                 <div class="car__img">
//                     <img
//                         src="' . htmlspecialchars($imgSrc, ENT_QUOTES, 'UTF-8') . '"
//                         alt="' . htmlspecialchars($row["title"], ENT_QUOTES, 'UTF-8') . '"
//                     />
//                 </div>
//                 <div class="car__short-info">
//                     <span class="price">' . htmlspecialchars($xfieldsArray['price'], ENT_QUOTES, 'UTF-8') . ' AZN</span>
//                     <span class="model">' . htmlspecialchars($xfieldsArray['brand'], ENT_QUOTES, 'UTF-8') . ' ' . htmlspecialchars($xfieldsArray['model'], ENT_QUOTES, 'UTF-8') . '</span>
//                     <div>
//                         <span class="issue-date">' . htmlspecialchars($xfieldsArray['year-of-car'], ENT_QUOTES, 'UTF-8') . ', </span>
//                         <span class="engine-volume">' . htmlspecialchars($xfieldsArray['engine-capacity'], ENT_QUOTES, 'UTF-8') . ' L,</span>
//                         <span class="mileage">' . htmlspecialchars($xfieldsArray['mileage'], ENT_QUOTES, 'UTF-8') . ' km,</span>
//                         <span class="color">' . htmlspecialchars($xfieldsArray['color'], ENT_QUOTES, 'UTF-8') . ',</span>
//                     </div>
//                     <span class="post-date">' . date("d/m/Y H:i", strtotime($row['date'])) . '</span>
//                 </div>
//             </a>';
//     }
// } else {
//     echo "<p>No results found for the specified criteria.</p>";
// }

// echo '</div>'; // Closing the cars__inner div

// // Close the prepared statement
// $stmt->close();

// // Close the database connection
// $conn->close();
// ?> -->