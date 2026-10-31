import logging
from aiogram import Bot, Dispatcher, types, F
from aiogram.filters import Command
from aiogram.types import WebAppInfo, InlineKeyboardMarkup, InlineKeyboardButton

# ضع توكن البوت الخاص بك هنا (تحصل عليه من BotFather)
TOKEN = "YOUR_BOT_TOKEN_HERE"

logging.basicConfig(level=logging.INFO)
bot = Bot(token=TOKEN)
dp = Dispatcher()

# خوارزمية حساب الرهان وتوزيع الأرباح (مثال توضيحي)
def calculate_bet_outcome(bet_amount: float, multiplier: float, win: bool) -> float:
    """
    خوارزمية حساب النتيجة بناءً على قيمة الرهان ومعامل المضاعفة
    """
    if win:
        return round(bet_amount * multiplier, 2)
    else:
        return 0.0

@dp.message(Command("start"))
async def cmd_start(message: types.Message):
    # زر لفتح تطبيق التاكا (Mini App)
    keyboard = InlineKeyboardMarkup(
        inline_keyboard=[
            [
                InlineKeyboardButton(
                    text="🎮 افتح تطبيق تاكا (Taka)",
                    web_app=WebAppInfo(url="https://your-mini-app-url.com")
                )
            ],
            [
                InlineKeyboardButton(text="💰 محفظتي", callback_data="wallet"),
                InlineKeyboardButton(text="ℹ️ قواعد اللعبة", callback_data="rules")
            ]
        ]
    )
    
    welcome_text = (
        f"أهلاً بك يا {message.from_user.first_name} في منصة **تاكا (Taka)** للألعاب والرهان الخوارزمي! 🚀\n\n"
        "اختر اللعبة وابدأ التحدي الآن مباشرة من داخل تلغرام."
    )
    await message.answer(welcome_text, reply_markup=keyboard, parse_mode="Markdown")

@dp.callback_query(F.data == "wallet")
async def process_wallet(callback: types.CallbackQuery):
    # خوارزمية جلب الرصيد (محاكاة)
    user_balance = 150.00  # مثال لرصيد المستخدم
    await callback.message.answer(f"💼 محفظتك الحالية:\nالرصيد: **{user_balance} USDT**", parse_mode="Markdown")
    await callback.answer()

if __name__ == "__main__":
    import asyncio
    asyncio.run(dp.start_polling(bot))

