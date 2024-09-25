# Hotel-industry-trending
## Giới thiệu về dữ liệu
Tập dữ liệu này chứa thông tin về đặt phòng khách sạn, bao gồm thông tin chi tiết về khách, đặt phòng của họ và các thuộc tính của khách sạn từ tháng 7 năm 2015 đến tháng 8 năm 2017
- **hotel**: The type of hotel, either "City Hotel" or "Resort Hotel."
- **is_canceled**: Binary value indicating whether the booking was cancelled (1) or not (0).
- **lead_time**: Number of days between booking and arrival.
- **arrival_date_year**: Year of arrival date.
- **arrival_date_month**: Month of arrival date.
- **arrival_date_week_number**: Week number of arrival date.
- **arrival_date_day_of_month**: Day of the month of arrival date.
- **stays_in_weekend_nights**: Number of weekend nights (Saturday or Sunday) the guest stays.
- **stays_in_week_nights**: Number of weekday nights (Monday to Friday) the guest stays.
- **adults**: Number of adults.
- **children**: Number of children.
- **babies**: Number of babies.
- **meal**: Type of meal booked.
- **country**: Country of origin.
- **market_segment**: Market segment designation.
- **distribution_channel**: Booking distribution channel.
- **is_repeated_guest**: Binary value indicating whether the guest is a repeated guest (1) or not (0).
- **previous_cancellations**: Number of previous booking cancellations.
- **previous_bookings_not_canceled**: Number of previous bookings not cancelled.
- **reserved_room_type**: Code of room type reserved.
- **assigned_room_type**: Code of room type assigned at check-in.
- **booking_changes**: Number of changes/amendments made to the booking.
- **deposit_type**: Type of deposit made.
- **agent**: ID of the travel agency.
- **company**: ID of the company.
- **days_in_waiting_list**: Number of days on the waiting list before booking.
- **customer_type**: Type of booking.
- **adr**: Average daily rate.
- **required_car_parking_spaces**: Number of car parking spaces required.
- **total_of_special_requests**: Number of special requests made.
- **reservation_status**: Reservation last status.
- **reservation_status_date**: Date of the last status.
- **name**: Guest's name. (Not Real)
- **email**: Guest's email address.(Not Real)
- **phone-number**: Guest's phone number. (Not Real)
- **credit_card**: Guest's credit card details. (Not Real)
## Kiểm tra và xử lý dữ liệu
### Tổng quan về dữ liệu
#### Dataset Overview
```
 #   Column                          Non-Null Count   Dtype  
---  ------                          --------------   -----  
 0   hotel                           119390 non-null  object 
 1   is_canceled                     119390 non-null  int64  
 2   lead_time                       119390 non-null  int64  
 3   arrival_date_year               119390 non-null  int64  
 4   arrival_date_month              119390 non-null  object 
 5   arrival_date_week_number        119390 non-null  int64  
 6   arrival_date_day_of_month       119390 non-null  int64  
 7   stays_in_weekend_nights         119390 non-null  int64  
 8   stays_in_week_nights            119390 non-null  int64  
 9   adults                          119390 non-null  int64  
 10  children                        119386 non-null  float64
 11  babies                          119390 non-null  int64  
 12  meal                            119390 non-null  object 
 13  country                         118902 non-null  object 
 14  market_segment                  119390 non-null  object 
 15  distribution_channel            119390 non-null  object 
 16  is_repeated_guest               119390 non-null  int64  
 17  previous_cancellations          119390 non-null  int64  
 18  previous_bookings_not_canceled  119390 non-null  int64  
 19  reserved_room_type              119390 non-null  object 
 20  assigned_room_type              119390 non-null  object 
 21  booking_changes                 119390 non-null  int64  
 22  deposit_type                    119390 non-null  object 
 23  agent                           103050 non-null  float64
 24  company                           6797 non-null  float64
 25  days_in_waiting_list            119390 non-null  int64  
 26  customer_type                   119390 non-null  object 
 27  adr                             119390 non-null  float64
 28  required_car_parking_spaces     119390 non-null  int64  
 29  total_of_special_requests       119390 non-null  int64  
 30  reservation_status              119390 non-null  object 
 31  reservation_status_date         119390 non-null  object 
 32  name                            119390 non-null  object 
 33  email                           119390 non-null  object 
 34  phone-number                    119390 non-null  object 
 35  credit_card                     119390 non-null  object 
```

### Xử lý dữ liệu
#### Dữ liệu thiếu
- Cột children thiếu 4 giá trị thay bằng 0 để giả định cho không có trẻ em 
- Cột country thiếu 488 giá trị thay bằng 'Unknown' để giả định không biết đến từ quốc gia nào
- Cột agent thiếu 16340 giá trị thay bằng 0 để giả định cho việc không rõ đặt phòng khách sạn đến từ đơn vị OTA nào
- Cột company thiếu 112593 giá trị thay bằng 0 để giả định cho việc không rõ đặt phòng đến từ Company nào
```
1  # Handling missing values as per the instructions
2 
3  # Replace missing values in 'children' with 0
4  hotel_booking_data['children'].fillna(0, inplace=True)
5
6  # Replace missing values in 'country' with 'Unknown'
7  hotel_booking_data['country'].fillna('Unknown', inplace=True)
8
9  # Replace missing values in 'agent' and 'company' with 0
10 hotel_booking_data['agent'].fillna(0, inplace=True)
11 hotel_booking_data['company'].fillna(0, inplace=True)
12 
13 # Check if the missing values are handled correctly
14 missing_values_after = hotel_booking_data.isnull().sum()
15 missing_values_after
```
#### Dữ liệu nhiễu
##### Dữ liệu ngoại lai
- Có 1 giá trị âm trong cột adr  (Average Daily Rate) trong bộ dữ liệu. adr đại diện cho giá trung bình cho mỗi phòng khách sạn được bán trong 1 ngày nhất định và không thể âm. 
- Market Segement 
##### Dữ liệu nhất quán
- Có 180 booking trong bộ dữ liệu mà không có khách (tức là số lượng adults, children, và babies đều bằng 0). 
- Có 223 booking trong bộ dữ liệu mà không có người lớn (adults = 0) nhưng lại có trẻ em (children > 0) hoặc trẻ sơ sinh (babies > 0). Đây có thể là một tình huống không hợp lý trong thực tế, vì thông thường không có trường hợp booking phòng chỉ với trẻ em mà không có người lớn đi kèm.
##### Dữ liệu không quan trọng
- Tất cả các giá trị của các cột: name, email, phone-number và credit_card đã được tạo một cách giả mạo bằng cách sử dụng python và được điền vào tập dữ liệu.
```
1 hotel_booking_data = hotel_booking_data[hotel_booking_data['adr'] >= 0]
2 hotel_booking_data = hotel_booking_data[hotel_booking_data['adults'] > 0]
3 hotel_booking_data = hotel_booking_data[hotel_booking_data['market_segment'] == 'uni]
4 
5 hotel_booking_data.drop(columns=['name', 'email', 'phone-number', 'credit_card'], inplace=True)
6
7 updated_shape = hotel_booking_data.shape
8 updated_shape
```
#### Dữ liệu trùng lập
- Không có dữ liệu trùng lập
```
1 # Check for duplicate rows in the first dataset
2 duplicate_rows = data.duplicated().sum()
```
#### Định dạng dữ liệu
- Phần lớn các cột có định dạng phù hợp với nội dung của chúng
- Cột reservation_status_date có thể cần được chuyển đổi sang định dạng ngày tháng.
- Cột children, agent và  companycó thể cần chuyển đổi sang định dạng số nguyên (int)
```
1  # Converting 'children', 'agent', and 'company' to integer data types
2  hotel_booking_data['children'] = hotel_booking_data['children'].astype(int)
3  hotel_booking_data['agent'] = hotel_booking_data['agent'].astype(int)
4  hotel_booking_data['company'] = hotel_booking_data['company'].astype(int)
5
6  # Converting 'reservation_status_date' to datetime data type
7  hotel_booking_data['reservation_status_date'] = pd.to_datetime(hotel_booking_data['reservation_status_date'])
8
9  # Checking the data types again to confirm the changes
10 updated_data_types = hotel_booking_data.dtypes
11 updated_data_types
```
## Phân tích dữ liệu
Để phân tích xu hướng của ngành du lịch, nhóm của tập trung vào một số yếu tố chính như:
1. Sự biến động theo mùa: Xem xét các xu hướng theo tháng và năm để xác định thời điểm cao điểm và thấp điểm trong ngành du lịch.
2. Phân khúc thị trường và kênh phân phối: Xem xét thị trường và kênh phân phối nào chủ yếu 
3. Loại khách hàng: Phân loại khách hàng (ví dụ: du lịch cá nhân, công việc, gia đình, nhóm) và xem xét xu hướng của từng nhóm.
4. Thời gian lưu trú: Phân tích thời gian lưu trú trung bình và xem xét sự khác biệt giữa các loại khách sạn.
5. Tỷ lệ hủy đặt phòng: Phân tích tỷ lệ hủy đặt phòng và xem xét các yếu tố có thể ảnh hưởng đến tỷ lệ này.
### Sự biến động theo mùa
Tiếp theo, hãy xem xét sự biến động theo mùa bằng cách phân tích số lượng đặt phòng theo tháng và năm. Điều này sẽ giúp ta hiểu rõ hơn về các mùa cao điểm và thấp điểm trong ngành du lịch. 
Biểu đồ cho thấy số lượng đặt phòng theo từng tháng và phân chia theo loại khách sạn. Từ biểu đồ, chúng ta có thể quan sát được một số điểm chính:
1. Biến động theo mùa: Cả hai loại khách sạn đều cho thấy sự biến động rõ rệt theo mùa. Có những đỉnh cao và thấp rõ ràng trong năm, phản ánh các mùa cao điểm và thấp điểm trong ngành du lịch.
2. Mùa cao điểm: Dường như có các đỉnh cao trong các tháng mùa hè (khoảng từ tháng 4 đến tháng 6), điều này phổ biến cho cả khách sạn thành phố và khách sạn nghỉ dưỡng. Điều này có thể phản ánh kỳ nghỉ mùa hè và sự tăng cường du lịch vào thời gian này.
3. Sự khác biệt giữa hai loại khách sạn: Khách sạn thành phố (City Hotel) có vẻ như có số lượng đặt phòng cao hơn so với khách sạn nghỉ dưỡng (Resort Hotel) trong hầu hết các thời điểm của năm.
### Phân khúc thị trường và kênh phân phối
- Biểu đồ này cho thấy sự phân bố số lượng đặt phòng theo các phân khúc thị trường khác nhau. Có thể thấy rõ ràng một số phân khúc thị trường như 'Online TA' (Travel Agent) chiếm ưu thế lớn.

- Biểu đồ trên thể hiện sự phân bố số lượng đặt phòng qua các kênh phân phối khác nhau. Rõ ràng, có một số kênh như 'TA/TO' (Travel Agents/Tour Operators) chiếm ưu thế lớn trong việc phân phối và bán phòng.

### Phân khúc khách hàng
Bây giờ, hãy phân tích loại khách hàng và xem xét xu hướng của từng nhóm. Chúng ta sẽ xem xét phân phối của các loại khách hàng và tỷ lệ hủy đặt phòng của từng loại. 

Phân tích về loại khách hàng cho thấy:
- Phân phối Loại Khách Hàng:
  - Transient: Là loại khách hàng phổ biến nhất với 89,337 đặt phòng.
  - Transient-Party: Đứng thứ hai với 25,005 đặt phòng.
  - Contract: Có 4,071 đặt phòng.
  - Group: Ít phổ biến nhất với 573 đặt phòng.
- Top Quốc Gia Có Khách Đến Nhiều Nhất
### Thời gian lưu trú
Bây giờ, hãy xem xét thời gian lưu trú trung bình và so sánh giữa các loại khách sạn. Điều này sẽ cung cấp thông tin về mô hình lưu trú của khách hàng tại các loại khách sạn khác nhau. 

Phân tích về thời gian lưu trú trung bình cho thấy:
- City Hotel: Thời gian lưu trú trung bình là khoảng 3 đêm.
- Resort Hotel: Thời gian lưu trú trung bình cao hơn, khoảng 4.3 đêm.
> Điều này cho thấy khách du lịch thường dành nhiều thời gian hơn tại các khách sạn nghỉ dưỡng so với khách sạn thành phố. Điều này có thể phản ánh mục đích chính của chuyến đi: nghỉ dưỡng tại khách sạn nghỉ dưỡng thường liên quan đến kỳ nghỉ dài hơn và thư giãn, trong khi khách sạn thành phố thường được sử dụng cho chuyến đi ngắn ngày hoặc công việc.
### Tỷ lệ huỷ phòng
- Tỷ lệ hủy phòng trong bộ dữ liệu này là khoảng 37.08%. Cụ thể, có 44,115 trường hợp hủy phòng và 74,871 trường hợp không hủy phòng.
  - Tỷ lệ hủy đặt phòng có sự khác biệt đáng kể giữa hai loại khách sạn:
  - City Hotel: Tỷ lệ hủy đặt phòng khoảng 41.8%.
  - Resort Hotel: Tỷ lệ hủy đặt phòng khoảng 27.8%.
> Phân tích này cho thấy khách sạn thành phố (City Hotel) có tỷ lệ hủy đặt phòng cao hơn so với khách sạn nghỉ dưỡng (Resort Hotel). Điều này có thể liên quan đến bản chất của các chuyến đi: chuyến đi đến thành phố thường linh hoạt hơn và có thể thay đổi hoặc hủy dễ dàng hơn so với kỳ nghỉ dài ngày tại khách sạn nghỉ dưỡng.

Một số yếu tố có thể ảnh hưởng đến tỷ lệ huỷ đặt phòng
  - Market Segment
    - Phân phối Đặt Phòng:
      - Online TA (Travel Agents): Là kênh đặt phòng phổ biến nhất với 56,221 đặt phòng.
      - Offline TA/TO (Travel Agents/Tour Operators): Đứng thứ hai với 24,179 đặt phòng.
      - Groups: Có 19,790 đặt phòng.
      - Các kênh khác như Direct, Corporate và Complementary cũng có số lượng đặt phòng đáng kể.
    - Tỷ Lệ Hủy Đặt Phòng
      - Groups: Có tỷ lệ hủy cao nhất là 61.1%.
      - Online TA và Offline TA/TO: Cũng có tỷ lệ hủy cao, lần lượt là 36.7% và 34.3%.
      - Các kênh như Direct, Corporate và Complementary có tỷ lệ hủy thấp hơn.
> Những kết quả này cho thấy rằng các nhóm đặt phòng qua các đại lý du lịch (Offline và Online) và đặt phòng nhóm (Groups) có tỷ lệ hủy cao. Điều này có thể phản ánh sự không chắc chắn hoặc thay đổi kế hoạch dễ dàng hơn khi đặt qua các kênh này
  - Room Type
    - Phân phối Đặt Phòng theo Loại Phòng:
      - Loại Phòng A: Là loại phòng được đặt nhiều nhất với 85,862 đặt phòng.
      - Loại Phòng D: Đứng thứ hai với 19,178 đặt phòng.
      - Các loại phòng khác như E, F, và G cũng có số lượng đặt phòng đáng kể.
    - Tỷ Lệ Hủy Đặt Phòng theo Loại Phòng:
      - Loại Phòng H: Có tỷ lệ hủy cao nhất là khoảng 40.8%.
      - Loại Phòng A và G: Cũng có tỷ lệ hủy tương đối cao, lần lượt là 39.2% và 36.4%.
      - Các loại phòng khác như B, C, D, E, và F có tỷ lệ hủy thấp hơn.
> Những kết quả này cho thấy rằng các loại phòng phổ biến nhất không nhất thiết có tỷ lệ hủy thấp nhất. Điều này có thể do nhu cầu cao và tính linh hoạt trong việc thay đổi hoặc hủy bỏ các loại phòng phổ biến.
  - Lead Time
    - Thời Gian Dẫn Đầu Ngắn (0-30 ngày): Tỷ lệ hủy đặt phòng trung bình là khoảng 24.0%. Điều này cho thấy khi đặt phòng gần với ngày đến, khách hàng có xu hướng duy trì lịch trình đặt phòng của họ.
    - Thời Gian Dẫn Đầu Trung Bình (31-180 ngày): Tỷ lệ hủy tăng lên đáng kể, khoảng 41.7%. Điều này có thể phản ánh sự không chắc chắn và khả năng thay đổi kế hoạch trong khoảng thời gian này.
    - Thời Gian Dẫn Đầu Dài (>180 ngày): Tỷ lệ hủy cao nhất, khoảng 62.2%. Điều này cho thấy rằng việc lên kế hoạch càng xa, khả năng hủy đặt phòng càng cao, có lẽ do thay đổi trong hoàn cảnh hoặc kế hoạch.
> Những kết quả này cho thấy rằng thời gian dẫn đầu đặt phòng là một yếu tố quan trọng ảnh hưởng đến tỷ lệ hủy đặt phòng. Khách hàng có xu hướng duy trì các đặt phòng được thực hiện gần với ngày đến, trong khi những đặt phòng được thực hiện sớm hơn có nguy cơ bị hủy cao hơn.
- Phân tích về yêu cầu đặc biệt và ảnh hưởng của chúng đến tỷ lệ hủy đặt phòng cho thấy:
  - Số Lượng Yêu Cầu Đặc Biệt Trung Bình:
    - Resort Hotel: Có số lượng yêu cầu đặc biệt trung bình cao hơn (khoảng 0.62 yêu cầu/đặt phòng) so với City Hotel.
    - City Hotel: Có khoảng 0.55 yêu cầu đặc biệt trung bình cho mỗi đặt phòng.
  - Tỷ Lệ Hủy Đặt Phòng theo Số Lượng Yêu Cầu Đặc Biệt:
    - Có sự giảm đáng kể về tỷ lệ hủy đặt phòng khi số lượng yêu cầu đặc biệt tăng lên. Đối với những đặt phòng không có yêu cầu đặc biệt, tỷ lệ hủy là khoảng 47.8%.
    - Khi tăng số lượng yêu cầu đặc biệt, tỷ lệ hủy giảm xuống, với chỉ 22% cho 1 hoặc 2 yêu cầu, và tiếp tục giảm xuống còn khoảng 10.7% cho 4 yêu cầu và 5% cho 5 yêu cầu.
> Những kết quả này cho thấy rằng khi khách hàng có nhiều yêu cầu đặc biệt hơn, họ có xu hướng duy trì lịch trình đặt phòng của mình, có lẽ do họ đã lên kế hoạch cụ thể và có nhu cầu cụ thể cho chuyến đi.
  - Deposit_type
    - Chi tiết tỷ lệ hủy đặt phòng theo loại tiền đặt cọc như sau:
      - Không Đặt Cọc ("No Deposit"):
        - Tỷ lệ hủy: 28.38%
        - Tổng số đặt phòng: 104,237
      - Không Hoàn Tiền ("Non Refund"):
        - Tỷ lệ hủy: 99.36%
        - Tổng số đặt phòng: 14,587
      - Hoàn Tiền ("Refundable"):
        - Tỷ lệ hủy: 22.22%
        - Tổng số đặt phòng: 162
> Điều này cho thấy: 
> - Loại "Không Hoàn Tiền" có tỷ lệ hủy đặt phòng cao nhất, gần 100%, có thể làm suy giảm lòng tin của khách hàng nếu họ muốn thay đổi hoặc hủy đặt phòng.
> - Mặc dù tỷ lệ hủy đặt phòng thấp hơn đáng kể đối với loại "Hoàn Tiền", số lượng đặt phòng thuộc loại này lại rất ít, có thể do tính chất đặc biệt của khách hàng lựa chọn chính sách này
> - Loại "Không Đặt Cọc" chiếm số lượng đặt phòng lớn nhất và có tỷ lệ hủy đặt phòng ở mức vừa phải.
