using System.ComponentModel.DataAnnotations;

namespace YourProject.Models
{
    public class PriceQuotationModel
    {
        [Required(ErrorMessage = "Subtotal is required")]
        [Range(0.01, double.MaxValue, ErrorMessage = "Subtotal must be greater than 0")]
        public decimal Subtotal { get; set; }

        [Required(ErrorMessage = "Discount percent is required")]
        [Range(0, 100, ErrorMessage = "Discount percent must be between 0 and 100")]
        public decimal DiscountPercent { get; set; }

        public decimal DiscountAmount => Subtotal * (DiscountPercent / 100);
        public decimal Total => Subtotal - DiscountAmount;
    }
}
using Microsoft.AspNetCore.Mvc;
using YourProject.Models;

namespace YourProject.Controllers
{
    public class PriceQuotationController : Controller
    {
        [HttpGet]
        public IActionResult Index()
        {
            return View(new PriceQuotationModel());
        }

        [HttpPost]
        public IActionResult Index(PriceQuotationModel model)
        {
            if (ModelState.IsValid)
            {
                return View(model);
            }
            return View(model);
        }

        [HttpPost]
        public IActionResult Clear()
        {
            return RedirectToAction("Index");
        }
    }
}
@model YourProject.Models.PriceQuotationModel
@{
    ViewData["Title"] = "Price Quotation";
}

<h1>Price Quotation</h1>

<form asp-action="Index" method="post">
    <div>
        <label>Subtotal:</label>
        <input asp-for="Subtotal" />
        <span asp-validation-for="Subtotal" class="text-danger"></span>
    </div>

    <div>
        <label>Discount percent:</label>
        <input asp-for="DiscountPercent" />
        <span asp-validation-for="DiscountPercent" class="text-danger"></span>
    </div>

    <div>
        <label>Discount amount:</label>
        <output>@Model.DiscountAmount.ToString("C")</output>
    </div>

    <div>
        <label>Total:</label>
        <output>@Model.Total.ToString("C")</output>
    </div>

    <button type="submit">Calculate</button>
    <button type="submit" formaction="/PriceQuotation/Clear">Clear</button>
</form>

<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.6.0.min.js"></script>
<script src="https://ajax.aspnetcdn.com/ajax/jquery.validate/1.19.3/jquery.validate.min.js"></script>
<script src="https://ajax.aspnetcdn.com/ajax/mvc/5.2.6/jquery.validate.unobtrusive.min.js"></script>

@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
