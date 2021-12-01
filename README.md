QRCode

Implementation QRCode with .net core web Application - C#

QRCoder is a simple library, written in C#.NET, which enables you to create QR codes. It hasn't any dependencies to other libraries and is available as .NET Framework and .NET Core PCL version on NuGet.

Installation

Either checkout this Github repository or install QRCoder via NuGet Package Manager. 
If you want to use NuGet just search for "QRCoder" or run the following command in the NuGet Package Manager console:

      PM> Install-Package QRCoder
      PM> Install-Package Microsoft.AspNetCore.Mvc.TagHelpers
      

Add this code to your HomeController

            [ValidateAntiForgeryToken]
            [HttpPost]
            public IActionResult Index(string txtQRCode)
            {
                QRCodeGenerator _qrCode = new QRCodeGenerator();
                QRCodeData _qrCodeData = _qrCode.CreateQrCode(txtQRCode, QRCodeGenerator.ECCLevel.Q);
                QRCode qrCode = new QRCode(_qrCodeData);
                Bitmap qrCodeImage = qrCode.GetGraphic(20);
                return View(BitmapToBytesCode(qrCodeImage));
            }
            
            [NonAction]
            private static Byte[] BitmapToBytesCode(Bitmap image)
            {
                using (MemoryStream stream = new MemoryStream())
                {
                    image.Save(stream, System.Drawing.Imaging.ImageFormat.Png);
                    return stream.ToArray();
                }
            }

  and this code to your index.cshtml

      @model Byte[]
      @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
      @{
          ViewData["Title"] = "Home Page";
      }
      <!DOCTYPE html>
      <meta name="viewport" content="width=device-width" />
      <title>Index</title>

     <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">

     @section Scripts{
      }

     <form asp-action="Index" asp-controller="Home" asp-antiforgery="true">
          <div class="container">
              <div class="panel-group">
                  <div class="panel panel-info">
                      <div class="panel-heading">Generate QR Code</div>
                      <div class="panel-body">
                          <div class="row">
                              <div class="col-md-12">
                                  <div class="col-md-3">Type text to generate QR Code</div>
                                  <div class="col-md-9"><input type="text" class="form-control" id="txtQRCode" name="txtQRCode" /></div>
                              </div>
                          </div>
                          <div class="row mt-3">
                              <div class="col-md-12">
                                  <div class="col-md-3"></div>
                                  <div class="col-md-9">
                                      <input type="submit" class="btn btn-primary" id="btnSubmit" value="Generate QR Code" autocomplete="off" />
                                  </div>
                              </div>
                          </div>
                          @{
                              if (Model != null)
                              {
                                  <div class="row mt-3">
                                      <div class="col-md-12">
                                          <div class="col-md-3"></div>
                                          <div class="col-md-9">
                                              <img src="@String.Format("data:image/png;base64,{0}", Convert.ToBase64String(Model))" height="300" width="300" />
                                          </div>
                                      </div>
                                  </div>
                              }
                          }
                      </div>
                  </div>
              </div>
          </div>
      </form>
